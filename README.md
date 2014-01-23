define command {
        command_name notify-service-by-flowdock
        command_line curl -XPOST -H "content-type:application/json" -d '{"content":"Notification Type: $NOTIFICATIONTYPE$\n\nService: $SERVICEDESC$\nHost: $HOSTALIAS$\nAddress: $HOSTADDRESS$\nState: $SERVICESTATE$\n\nAdditional Info:\n\n$SERVICEOUTPUT$","external_user_name": "Nagios"}' https://api.flowdock.com/v1/messages/chat/YOUR_FLOW_API_TOKEN
}

define command {
        command_name notify-host-by-flowdock
        command_line curl -XPOST -H "content-type:application/json" -d '{"content":"$NOTIFICATIONTYPE$ $HOSTSTATE$ Host($HOSTALIAS$) Info($HOSTOUTPUT$)","external_user_name": "Nagios"}' https://api.flowdock.com/v1/messages/chat/YOUR_FLOW_API_TOKEN
}

define contact{
        contact_name                    flowdock
        host_notifications_enabled      1
        service_notifications_enabled   1
        host_notification_period        24x7
        service_notification_period     24x7
        host_notification_options       d,u,r,f,s
        service_notification_options    w,u,c,r,f,s
        host_notification_commands      notify-host-by-flowdock
        service_notification_commands   notify-service-by-flowdock
}
