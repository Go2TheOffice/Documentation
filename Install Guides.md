# Install Guides

This document should contain install instructions (as well as some troubleshooting) for several services installed on the server

Password for guac vm: #TODO: DELETE THIS FROM THIS DOC guacamole456)

## Guacamole
The easiest way to install guacamole was to use an [install script](https://github.com/MysticRyuujin/guac-install/). This script sets up a few things:
1. Misc. dependencies
2. Tomcat server
3. MySQL server
4. TOTP
5. Guacamole server, client and extensions
6. Sets everything up and connects all components

The core of the script is here:
```
SERVER="http://apache.org/dyn/closer.cgi?action=download&filename=guacamole/${GUACVERSION}"
echo -e "${BLUE}Downloading files...${NC}"

# Download Guacamole Server
wget -q --show-progress -O guacamole-server-${GUACVERSION}.tar.gz ${SERVER}/source/guacamole-server-${GUACVERSION}.tar.gz
if [ $? -ne 0 ]; then
    echo -e "${RED}Failed to download guacamole-server-${GUACVERSION}.tar.gz" 1>&2
    echo -e "${SERVER}/source/guacamole-server-${GUACVERSION}.tar.gz${NC}"
    exit 1
else
    # Extract Guacamole Files
    tar -xzf guacamole-server-${GUACVERSION}.tar.gz
fi
echo -e "${GREEN}Downloaded guacamole-server-${GUACVERSION}.tar.gz${NC}"

# Download Guacamole Client
wget -q --show-progress -O guacamole-${GUACVERSION}.war ${SERVER}/binary/guacamole-${GUACVERSION}.war
if [ $? -ne 0 ]; then
    echo -e "${RED}Failed to download guacamole-${GUACVERSION}.war" 1>&2
    echo -e "${SERVER}/binary/guacamole-${GUACVERSION}.war${NC}"
    exit 1
fi
echo -e "${GREEN}Downloaded guacamole-${GUACVERSION}.war${NC}"
```

This script is 678 lines long, and looks like a real bitch to figure out manually. May be a good idea to make a copy somewhere in case the repo goes down. 

