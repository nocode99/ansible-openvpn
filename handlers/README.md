Handlers can be called from playbooks.  They are useful for starting/stopping/restarting services if you need to call multiple times.  Say you update a conf file in a playbook, you can notify a handler to restart the service.  