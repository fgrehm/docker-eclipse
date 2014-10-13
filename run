#!/bin/bash

# Make sure the user data directory is owned by the developer user
if [ -d /home/developer/.eclipse ]; then
  sudo chown developer:developer /home/developer/.eclipse
fi
exec /opt/eclipse/eclipse
