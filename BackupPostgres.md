#!/bin/sh
#bkp_faculdade.sh
#DATA iria imprimir a data: ano-mes-dia.
DATA='/bin/date+%Y-%m-%d'
•	#NOME armazena o nome do arquivo de BACKUP
•	#O Diretório onde o arquivo será salvo neste caso é:
#/www/virtual/backup é uma pasta publica do apache.
•	#Coloque o diretorio onde quiser guardar o BACKUP.
NOME="/www/virtual/backup/faculdade.sh-$DATA.sql"
•	#Variaveis do POSTGRESQL
HOST="localhost"
USER="postgres"
PASSWORD="12345"
DATABASE="faculdade.sh"
pg_dump -h $localhost -u $postgres -p $12345 $faculdade.sh > $faculdade.backup
