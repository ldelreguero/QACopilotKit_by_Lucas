# Browser apis: geolocation, permissions & more

## Table of contents

1. [Geolocation](#geolocation)
2. [Permissions](#permissions)
3. [Clipboard](#clipboard)
4. [Notifications](#notifications)
5. [Camera & Microphone](#camera--microphone)

## Geolocation

### Mock location

```typescript
test("shows nearby stores", async ({ context }) => {
  // Grant permission and set location
  await context.grantPermissions(["geolocation"]);
  await context.setGeolocation({ latitude: 37.7749, longitude: -122.4194 }); // San Francisco

  const page = await context.newPage();
  await page.goto("/store-finder");
  await page.getByRole("button", { name: "Find Nearby" }).click();

  await expect(page.getByText("San Francisco")).toBeVisible();
});
```

### Geolocation fixture

```typescript
// fixtures/geolocation.fixture.ts
import { test as base } from "@playwright/test";

type Coordinates = { latitude: number; longitude: number; accuracy?: number };

type GeoFixtures = {
  setLocation: (coords: Coordinates) => Promise<void>;
};

export const test = base.extend<GeoFixtures>({
  setLocation: async ({ context }, use) => {
    await context.grantPermissions(["geolocation"]);

    await use(async (coords) => {
      await context.setGeolocation({
        latitude: coords.latitude,
        longitude: coords.longitude,
        accuracy: coords.accuracy ?? 100,
      });
    });
  },
});

// Usage
test("delivery zone check", async ({ page, setLocation }) => {
  await setLocation({ latitude: 40.7128, longitude: -74.006 }); // NYC

  await page.goto("/delivery");

  await expect(page.getByText("Delivery available")).toBeVisible();
});
```

### Test location changes

```typescript
test("tracks location updates", async ({ context }) => {
  await context.grantPermissions(["geolocation"]);

  const page = await context.newPage();
  await page.goto("/tracking");

  // Initial location
  await context.setGeolocation({ latitude: 37.7749, longitude: -122.4194 });
  await page.getByRole("button", { name: "Start Tracking" }).click();

  await expect(page.getByTestId("location")).toContainText("37.7749");

  // Move to new location
  await context.setGeolocation({ latitude: 37.8044, longitude: -122.2712 });

  // Trigger location update
  await page.evaluate(() => {
    navigator.geolocation.getCurrentPosition(() => {});
  });

  await expect(page.getByTestId("location")).toContainText("37.8044");
});
```

### Test geolocation denial

```typescript
test("handles location denied", async ({ browser }) => {
  // Create context without geolocation permission
  const context = await browser.newContext({
    permissions: [], // No permissions
  });

  const page = await context.newPage();
  await page.goto("/store-finder");
  await page.getByRole("button", { name: "Find Nearby" }).click();

  await expect(page.getByText("Location access denied")).toBeVisible();
  await expect(page.getByLabel("Enter ZIP code")).toBeVisible();

  await context.close();
});
```

## Permissions

### Grant permissions

```typescript
test("notifications with permission", async ({ context }) => {
  await context.grantPermissions(["notifications"]);

  const page = await context.newPage();
  await page.goto("/alerts");

  // Notification API should work
  const permission = await page.evaluate(() => Notification.permission);
  expect(permission).toBe("granted");
});
```

### Test permission denied

```typescript
test("handles notification permission denied", async ({ browser }) => {
  const context = await browser.newContext({
    permissions: [], // Deny all
  });

  const page = await context.newPage();
  await page.goto("/notifications");

  await page.getByRole("button", { name: "Enable Notifications" }).click();

  await expect(page.getByText("Please enable notifications")).toBeVisible();

  await context.close();
});
```

### Multiple permissions

```typescript
test("video call with permissions", async ({ context }) => {
  await context.grantPermissions(["camera", "microphone", "notifications"]);

  const page = await context.newPage();
  await page.goto("/video-call");

  // All permissions should be granted
  const permissions = await page.evaluate(async () => ({
    camera: await navigator.permissions.query({
      name: "camera" as PermissionName,
    }),
    microphone: await navigator.permissions.query({
      name: "microphone" as PermissionName,
    }),
  }));

  expect(permissions.camera.state).toBe("granted");
  expect(permissions.microphone.state).toBe("granted");
});
```

## Clipboard

### Test copy to clipboard

```typescript
test("copy button works", async ({ page, context }) => {
  // Grant clipboard permissions
  await context.grantPermissions(["clipboard-read", "clipboard-write"]);

  await page.goto("/share");

  await page.getByRole("button", { name: "Copy Link" }).click();

  // Read clipboard content
  const clipboardContent = await page.evaluate(() =>
    navigator.clipboard.readText(),
  );

  expect(clipboardContent).toContain("https://example.com/share/");
});
```

### Test paste from clipboard

```typescript
test("paste from clipboard", async ({ page, context }) => {
  await context.grantPermissions(["clipboard-read", "clipboard-write"]);

  await page.goto("/editor");

  // Write to clipboard
  await page.evaluate(() => navigator.clipboard.writeText("Pasted content"));

  // Trigger paste
  await page.getByLabel("Content").focus();
  await page.keyboard.press("Control+V");

  await expect(page.getByLabel("Content")).toHaveValue("Pasted content");
});
```

### Clipboard fixture

```typescript
// fixtures/clipboard.fixture.ts
import { test as base } from "@playwright/test";

type ClipboardFixtures = {
  clipboard: {
    write: (text: string) => Promise<void>;
    read: () => Promise<string>;
  };
};

export const test = base.extend<ClipboardFixtures>({
  clipboard: async ({ page, context }, use) => {
    await context.grantPermissions(["clipboard-read", "clipboard-write"]);

    await use({
      write: async (text) => {
        await page.evaluate((t) => navigator.clipboard.writeText(t), text);
      },
      read: async () => {
        return page.evaluate(() => navigator.clipboard.readText());
      },
    });
  },
});
```

## Notifications

### Mock notification API

```typescript
test("shows browser notification", async ({ page }) => {
  const notifications: any[] = [];

  // Mock Notification constructor
  await page.addInitScript(() => {
    (window as any).__notifications = [];
    (window as any).Notification = class {
      constructor(title: string, options?: NotificationOptions) {
        (window as any).__notifications.push({ title, ...options });
      }
      static permission = "granted";
      static requestPermission = async () => "granted";
    };
  });

  await page.goto("/alerts");
  await page.getByRole("button", { name: "Notify Me" }).click();

  // Check notification was created
  const created = await page.evaluate(() => (window as any).__notifications);
  expect(created).toHaveLength(1);
  expect(created[0].title).toBe("New Alert");
});
```

### Test notification click

```typescript
test("notification click handler", async ({ page }) => {
  await page.addInitScript(() => {
    (window as any).Notification = class {
      onclick: (() => void) | null = null;
      constructor(title: string) {
        // Simulate click after creation
        setTimeout(() => this.onclick?.(), 100);
      }
      static permission = "granted";
      static requestPermission = async () => "granted";
    };
  });

  await page.goto("/messages");
  await page.evaluate(() => {
    new Notification("New Message");
  });

  // Should navigate to messages when notification clicked
  await expect(page).toHaveURL(/\/messages/);
});
```

## Camera & microphone

### Mock media devices

```typescript
test("video preview works", async ({ page, context }) => {
  await context.grantPermissions(["camera"]);

  // Mock getUserMedia
  await page.addInitScript(() => {
    navigator.mediaDevices.getUserMedia = async () => {
      const canvas = document.createElement("canvas");
      canvas.width = 640;
      canvas.height = 480;
      return canvas.captureStream();
    };
  });

  await page.goto("/video-settings");
  await page.getByRole("button", { name: "Start Camera" }).click();

  await expect(page.getByTestId("video-preview")).toBeVisible();
});
```

### Test media device selection

```typescript
test("switch camera", async ({ page }) => {
  await page.addInitScript(() => {
    navigator.mediaDevices.enumerateDevices = async () =>
      [
        {
          deviceId: "cam1",
          kind: "videoinput",
          label: "Front Camera",
          groupId: "1",
        },
        {
          deviceId: "cam2",
          kind: "videoinput",
          label: "Back Camera",
          groupId: "2",
        },
      ] as MediaDeviceInfo[];

    navigator.mediaDevices.getUserMedia = async () => {
      const canvas = document.createElement("canvas");
      return canvas.captureStream();
    };
  });

  await page.goto("/camera");

  // Should show camera options
  await expect(page.getByRole("combobox", { name: "Camera" })).toBeVisible();
  await expect(page.getByText("Front Camera")).toBeVisible();
  await expect(page.getByText("Back Camera")).toBeVisible();
});
```

### Test media errors

```typescript
test("handles camera access error", async ({ page }) => {
  await page.addInitScript(() => {
    navigator.mediaDevices.getUserMedia = async () => {
      throw new DOMException("Permission denied", "NotAllowedError");
    };
  });

  await page.goto("/video-call");
  await page.getByRole("button", { name: "Join Call" }).click();

  await expect(page.getByText("Camera access denied")).toBeVisible();
  await expect(
    page.getByRole("button", { name: "Join Audio Only" }),
  ).toBeVisible();
});
```

## Anti-Patterns to avoid

| Anti-Pattern                  | Problem                           | Solution                            |
| ----------------------------- | --------------------------------- | ----------------------------------- |
| Not granting permissions      | Tests fail with permission errors | Use `context.grantPermissions()`    |
| Testing real geolocation      | Flaky, environment-dependent      | Mock with `setGeolocation()`        |
| Not testing permission denial | Misses error handling             | Test both granted and denied states |
| Using real camera/mic         | CI has no devices                 | Mock `getUserMedia`                 |

## Related references

- **Fixtures**: See [fixtures-hooks.md](fixtures-hooks.md) for context fixtures
- **Mobile**: See [mobile-testing.md](mobile-testing.md) for device emulation
