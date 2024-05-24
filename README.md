# Tools

Tools I use.

## Notifications

To show notifications on screen you need `libnotify` and a notification daemon.

## Troubleshooting

If notifications are not shown, your system probably cannot startup the notification daemon through dbus. So, add the following configuration to D-Bus services directory (`/usr/share/dbus-1/services` or `$HOME/.local/share/dbus-1/services`):

    # org.freedesktop.Notifications.service
    [D-BUS Service]
    Name=org.freedesktop.Notifications
    Exec=/path/to/your/notification-daemon

