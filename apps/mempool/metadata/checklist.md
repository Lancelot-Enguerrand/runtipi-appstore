## Dynamic compose for mempool
This is a mempool update for using dynamic compose.
##### Reaching the app :
- [ ] http://localip:port
- [ ] https://mempool.tipi.local
##### In app tests :
- [ ] 📝 Register and log in
- [ ] 🖱 Basic interaction
- [ ] 🌆 Uploading data
- [ ] 🔄 Check data after restart
##### Volumes mapping :
- [ ] ${BITCOIND_DIR:-${APP_DATA_DIR}/../bitcoind/data}:/data/.bitcoin:ro
- [ ] ${APP_DATA_DIR}/data:/backend/cache
- [ ] ${APP_DATA_DIR}/data/db:/var/lib/mysql
##### Specific instructions :
- [ ] 🌳 Environment
- [ ] ⌨ Command
- [ ] 🔗 Depends on
- [ ] 👼 Stop grace period
- [ ] 👤 User (0:0)
