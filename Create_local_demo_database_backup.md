# Create a local demo database and backup (windows)

## Session one: write updating database cases

write update case:

	1.Add file: /clarify-hospital/common/models/IOSHealthRecords.json

	2.Add code to file /clarify-hospital/database/tables/create_tables.sql

	`CREATE TABLE IF NOT EXISTS ios_health_records (
	  id uuid,
	  patient_id uuid NOT NULL,
	  record jsonb NOT NULL,
	  PRIMARY KEY (record)
	);`

	3.Add code to file /clarify-hospital/database/tables/drop_tables.sql

	`DROP TABLE IF EXISTS ios_health_records CASCADE;`

## Session two: create and set new database

### Database name is hospital, install Postgresql in computer first. 

### Set ssl=on and add certificates(server.key/server.crt/root.crt) to $PGDATA server directory, reference document link(in chinese): https://blog.csdn.net/zhu4674548/article/details/71248365
	server.key //Private Key
		Generate server.key:
		`openssl genrsa -des3 -out server.key 1024`
			detail:
				genrsa //Generate an RSA private key
				-des3 //The options encrypt the private key with specified cipher before outputting it
				-out server.key //Output the key to the specified file
				numbits //The size of the private key to generate in bits
			Enter pass phrase and confirm it, then remove the pass phrase
		Remove pass phrase:
		`openssl rsa -in server.key -out server.key`
			detail:
				rsa //RSA key management
				-in filename //This specifies the input filename to read a key from
				-out filename //This specifies the output filename to write a key to
			Enter pass phrase
		Change permission and owner:
		`chmod 400 server.key
		 chown ec2-user:ec2-user server.key`
	server.crt //Server certificate
		`openssl req -new -key server.key -days 3650 -out server.crt -x509 -subj '/C=US/ST=California/L=PaloAlto/O=Clarify/CN=clarifyhealht.com/emailAddress=example@clarifyhealth.com'`
			detail:
				req //Certificate request and certificate generating utility
				-new //This option generates a new certificate request
				-key filename //This specifies the file to read the private key from
				-days n //When the -x509 option is being used this specifies the number of days to certify the certificate for
				-out filename //This specifies the output filename to write to
				-x509 //This option outputs a self signed certificate instead of a certificate request
				-subj arg //Sets subject name for new request or supersedes the subject name when processing a request

	root.crt //受信任的根证书
		Get a self signed certificate by copying the server.crt and renaming it as 'root.crt'
		`cp server.crt root.crt`
	Reset postgresql.conf file:
		ssl=on
		ssl_ca_file='root.crt'
	Reset pg_hba.conf file, add ssl connection principle:
		TYPE     DATABASE        USER            CIPR-ADDRESS            METHOD
		'hostssl all             all             0.0.0.0/0               md5'
	Restart server(start/restart/status cmd): 
		`pg_ctl -D "C:\Program Files\PostgreSQL\10\data" start // restart // status`


### 1.	Synchronize code, get data package: minimal2.backup.gz and install npm libs
`git pull -f https://github.com/clarifyhealth/clarify-hospital.git
cd clarify-hospital
npm install`

### 2. Force disconnection of all clients connected to this database if existed
 `psql -c "SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'hospital';"`

### 3. Drop and create database hospital;
`psql -c "drop database hospital" -U postgres
 psql -c "create database hospital;" -U postgres`

### 4. Export environment
`export DB=hospital HOSPITAL_HOME=/c/workspace/clarify-hospital
 export DATABASE_URL=postgres://clarify:IaCA61RaFsA74GeR@localhost:5432/$DB
 export DATABASE_ADMIN_URL=postgres://clarify:IaCA61RaFsA74GeR@localhost:5432/$DB`

### 5. Gzip minimal data to hospital database
`cat clarify-hospital/data/database/create_roles.sql | psql -U postgres -d hospital --set db=hospital --set role=clarify
 gzip -cd clarify-hospital/data/database/minimal2.backup.gz | psql -U postgres -d hospital`

### 6. Populate dev db, give execution permission to the files in /clarify-hospital/database folder by `chmod 777 -R clarify-hospital/database/*`
`./clarify-hospital/database/scripts/load_app_data_client.sh minimal`

### 7. Run upgrade and migrade case to update data in database
`cd /c/workspace/clarify-hospital/
database/generate/generate_one.sh "New Target Price Analysis DDL.sql"
node scripts/upgrade-database.js
node scripts/migrate-data.js`

### connect database by using this path:
	"localdb":"postgres://clarify:IaCA61RaFsA74GeR@localhost:5432/hospital?ssl=true"

### 8. Finally run test case to recreate patients' journey
`
ALLOW_HTTP=true nohup node server/server.js > /dev/null 2>&1 &
sleep 5
node_modules/.bin/newman run test/postman/mock_journeys/Mock_Journeys.postman_collection.json --environment test/postman/mock_journeys/Mock_Journeys.postman_environment.json  --bail
`

### Kill server process on port 3000
`netstat -ano | findstr :yourPortNumber
 taskkill /PID typeyourPIDhere /F`
or `taskkill /F /IM node.exe` or `fuser -k 3000/tcp` in linux


## Session three: backup
	pg_dump -d hospital > data/database/minimal2_ios_health_records.backup
	gzip data/database/minimal2_ios_health_records.backup

	Get a .gz extension name package, finished
