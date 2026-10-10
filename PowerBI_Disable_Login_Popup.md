# Power BI Desktop — Disable the Login Popup

## Objective

Prevent the recurring **"Enter your email address"** popup when using Power BI Desktop without a work or school account.

This procedure involves modifying a Windows Registry setting.

## 1. Close Power BI Desktop

1. Save any reports currently being edited.
2. Completely close Power BI Desktop.

## 2. Open the Windows Registry Editor

1. Press `Windows + R`.
2. Type `regedit`.
3. Press **Enter**.

## 3. Create the Power BI Desktop Registry Key

Navigate to the following path:

```text
HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft
```

If the `Microsoft Power BI Desktop` key does not exist:

1. Right-click the `Microsoft` key.
2. Select **New → Key**.
3. Name the new key:

```text
Microsoft Power BI Desktop
```

The complete Registry path should now be:

```text
HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Microsoft Power BI Desktop
```

## 4. Create the Registry Value

1. Select the `Microsoft Power BI Desktop` key.
2. Right-click in the right-hand panel.
3. Select **New → DWORD (32-bit) Value**.
4. Name the new value:

```text
ShowLeadGenDialog
```

5. Double-click the newly created value.
6. Set **Value data** to `0`.
7. Click **OK**.

### Expected Configuration

| Setting | Value |
|---|---|
| Registry key | `Microsoft Power BI Desktop` |
| Value name | `ShowLeadGenDialog` |
| Value type | `REG_DWORD` |
| Value data | `0` |

## 5. Restart Power BI Desktop

1. Close the Registry Editor.
2. Launch Power BI Desktop.
3. Open an existing report.
4. Check whether the login popup still appears while working.

## 6. Revert the Changes

To restore the previous configuration:

1. Open `regedit`.
2. Navigate to the `Microsoft Power BI Desktop` key.
3. Delete only the `ShowLeadGenDialog` value created earlier.
4. Restart Power BI Desktop.

## Notes

- This procedure does not require a work or school account.
- It does not disable local report creation or editing.
- Online features may still require authentication.
- The effectiveness of this setting may vary depending on the Power BI Desktop version.
- Avoid modifying unrelated Windows Registry settings.