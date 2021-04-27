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
         
          
## Creación del archivo manifiesto, este archivo contiene como estan escruturadas la lecturas 

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
 
         qiime tools import --type 'SampleData[PairedEndSequencesWithQuality]' --input-path manifest.csv --output-path raw-seqs.qza --input-format PairedEndFastqManifestPhred33




