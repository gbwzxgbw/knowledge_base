    netstat -lnp | grep 809 | awk '{print $7}' | awk 'BEGIN{FS="/"}{print $1}' | xargs kill
