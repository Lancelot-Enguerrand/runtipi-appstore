## Dynamic compose for guacamole
This is a guacamole update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://guacamole.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] ${APP_DATA_DIR}/data/db:/var/lib/postgresql/data
- [ ] ${APP_DATA_DIR}/data/init/sql:/config/sql
- [ ] ${APP_DATA_DIR}/data/init/init-guacamole.sh:/docker-entrypoint-initdb.d/init-guacamole.sh
##### Specific instructions :
- [ ] 🌳 Environment
