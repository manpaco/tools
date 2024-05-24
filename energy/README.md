# Energy scripts

This scripts should be called from `cron` or systemd timers.

## Systemd timers

Use this template as a starting point:

    # /etc/systemd/system/script-runner.timer or
    # ~/.config/systemd/user/script-runner.timer
    [Unit]
    Description=Run 'script' every minute
    
    [Timer]
    OnCalendar=*-*-* *:*:00
    
    [Install]
    WantedBy=default.target

Also you need a service file (with the same name and '.service' extension):

    # /etc/systemd/system/script-runner.service or
    # ~/.config/systemd/user/script-runner.service
    Unit]
    Description=Run 'script'
    
    [Service]
    ExecStart=script param1

Finally enable the timer (add `--user` for user instance):

    systemctl [--user] enable --now script-runner.timer

## Cron

Open the crontab editor:

    sudo crontab -e # for system wide
    crontab -e # for user instance

and use this as template:

    # run every 5 minutes
    */5 * * * * $HOME/.local/bin/your-script param1

## Troubleshooting

If you have problems with computer shutdown when using `cron` (first make sure you have `polkit` installed and configured) use the following configuration file for your `polkit`:

    # /etc/polkit-1/rules.d/20-allow-power-off.rules
    polkit.addRule(function(action, subject) {
        if(action.id == "org.freedesktop.login1.power-off" && subject.user == "<youruser>") {
            return polkit.Result.YES;
        }
    });

The problem is caused because the cron service is not part of an active session and therefore has no privileges to shutdown. With systemd you should have no problems.
