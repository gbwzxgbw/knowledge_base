# Tutorials
    http://www.grymoire.com/Unix/Awk.html

# Examples
    netstat -lnp | grep 809 | awk '{print $7}' | awk 'BEGIN{FS="/"}{print $1}' | xargs kill
