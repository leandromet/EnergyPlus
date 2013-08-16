#! /bin/bash -l

#{
export PATH="/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin"

CaminhoUser="ENERGY_7_2/BIONDO"
MailUser="lbiondo@cte.com.br"

caminho="/home/cte/SIMULA/$CaminhoUser"
echo "Pasta raiz de execução: $caminho"
cd $caminho/ENTRADA


horasim=$(date)
echo "$horasim"
sleep 1
ls *.idf > arquivos_idf
datasim=$(date +%y%m%d%H%M)


fileaq="$caminho/ENTRADA/arquivos_idf"
if [ -s $fileaq ]
 then
  mkdir $caminho/ENTRADA/resultado_$datasim
else
  echo "
!! Não foram encontrados arquivos IDF para simular
"
  exit 1
fi;


echo "data $datasim"
num=0

chmod 777 *.idf

for file in `cat arquivos_idf`;do
mkdir $caminho/ENTRADA//resultado_$datasim/$datasim-$num-${file%%.*}; mv $caminho/ENTRADA/$file $caminho/ENTRADA//resultado_$datasim/$datasim-$num-${file%%.*} ;

echo "A simular $file, resultado em $caminho/RESULTADOS/resultado_$datasim/$datasim-$num-${file%%.*}";  ((num++)); done

echo "sh $caminho/ENTRADA/repw_processos.sh /usr/local/EnergyPlus-7-2-0/bin/runenergyplus" > arquivos_idf
sort $caminho/ENTRADA/arquivo_clima.txt >> $caminho/ENTRADA/arquivos_idf
find $caminho/ENTRADA/resultado_$datasim/ -name *.idf > $caminho/ENTRADA/idf_plano
sort  $caminho/ENTRADA/idf_plano >> $caminho/ENTRADA/arquivos_idf
tr '\n' ' ' < $caminho/ENTRADA/arquivos_idf > $caminho/ENTRADA/idf_plano

sh $caminho/ENTRADA/idf_plano

cp $caminho/ENTRADA/arquivo_clima.txt $caminho/ENTRADA/resultado_$datasim/
cp $caminho/ENTRADA/idf_plano $caminho/ENTRADA/resultado_$datasim/simulacao_$datasim.txt

tar -cvf $datasim.tar resultado_$datasim/
gzip $datasim.tar

mv $datasim.tar.gz $caminho/ZRESULTADOS_BACKUP/
mv ./resultado_$datasim/ $caminho/RESULTADOS/resultado_${file%%.*}_$datasim


SUBJECT="Simulacao $datasim $file"
datasim=$(date)
# Email To ?
EMAIL="$MailUser"
# Email text/message
EMAILMESSAGE="$caminho/ENTRADA/emailmessage.txt"
echo "Simulacao Concluida"> $EMAILMESSAGE
echo "A simulacao com EnergyPlus 7.2 $file, iniciada em $horasim foi concluida em $datasim. Os resultados estao na pasta \\10.0.2.242\SIMULA\$CaminhoUser\RESULTADOS">>$EMAILMESSAGE

# send an email using /usr/bin/mailx
 /bin/mail -s "$SUBJECT" -S smtp-use-starttls -S ssl-verify=ignore -S smtp=smtp://smtp.gmail.com:587 -S smtp-auth=login -S smtp-auth-user=simula.cte@gmail.com -S smtp-auth-password=inteligencia360 -S from=cte "$EMAIL" < $EMAILMESSAGE

cd $caminho/RESULTADOS
chmod -R 777 *




exit 0
#} > $caminho/saidas_modelos
