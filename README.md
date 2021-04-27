## Qiime

## Activación de entorno 

Descarga de archivo manifiesto 

        wget https://data.qiime2.org/distro/core/qiime2-2020.2-py36-linux-conda.yml
        

Creación del entorno con conda 

        conda env create -n qiime2 --file qiime2-2019.1-py36-linux-conda.yml
        
Activación del entorno 

          source activate qiime2
          
## Subir metadatos

Subir los metadatos "Ojo" crear esta tabla en nano y eliminar todos los espacios de la tabla, sale error eliminar los espacios 

         qiime tools inspect-metadata metadata.tsv

Ejemplo de metadata.csv 

        #SampleID   Origen    Country   Year    Seson
        Sample1     Water     Mexico    2019    Spring
        Sample2     Soil      Mexico    2019    Spring


Opcional si sale error eliminar los espacios con sed en la terminal de la tabla 

          sed 's/ //g' metadata.tsv  
         
          
## Creación del archivo manifiesto
Este archivo contiene las lecturas 

        nano manifest.csv 
        
 Ejemplo de manifest.csv
          
          sample-id,adsolute-filepath,direction
          sample1,$PWD/sample1/read_R1.fastq.gz/forward
          sample1,$PWD/sample1/read_R2.fastq.gz/reverse
          sample2,$PWD/sample2/read_R1.fastq.gz/forward
          sample2,$PWD/sample2/read_R2.fastq.gz/reverse
          
  Opcional si sale error eliminar los espacios con sed en la terminal de la tabla 

          sed 's/ //g' metadata.tsv  
          
          
 ## Importación del manifiesto
 Aquí se importan las librarias 
 
         qiime tools import --type 'SampleData[PairedEndSequencesWithQuality]' --input-path manifest.csv --output-path raw-seqs.qza --input-format PairedEndFastqManifestPhred33


## Desreplicación y QC con dada2
        
        qiime dada2 denoise-paired --i-demultiplexed-seqs raw-seqs.qza --p-trunc-len-f 250 --p-trunc-len-r 250 --p-trunc-q 10 --o-representative-sequences rep-seqs.qza --o-table rep-table.qza --output-dir rep
        
        
## Descarga de la base de datos
Diferentes base de datos 
        
        wget -O "gg-13-8-99-515-806-nb-classifier.qza" "https://data.qiime2.org/2019.4/common/gg-13-8-99-515-806-nb-classifier.qza"
        
Cambiar de nombre 

        mv gg-13-8-99-515-806-nb-classifier.qza gg13_16S.qza
 
 Aquí se debe descargar la versión de la base de datos actual para no tener errores, por lo tanto el link anterior puede cambiar 
        
     
## Asignación taxonómica

Se realiza la asignación taxonómica 

        qiime feature-classifier classify-sklearn --i-classifier gg13_16S.qza --i-reads rep-seqs.qza --o-classification taxonomy.qza
 
Selección del Nivel taxonómico 
 
         qiime taxa collapse --i-table table.qza  --i-taxonomy taxonomy.qza --p-level 6 --output-dir taxtable
         

## Exportación de datos

                
       qiime metadata tabulate --m-input-file taxonomy.qza --o-visualization taxonomy.qzv
       
Gráfico de barras

       qiime taxa barplot --i-table rep-table.qza --i-taxonomy taxonomy.qza --m-metadata-file metadata.tsv --o-visualization taxa-barplot.qzv

         



     








 





