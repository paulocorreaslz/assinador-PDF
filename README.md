# Versão alterada por 
``` 
Paulo Jose Melo Gomes Correa - 2017 - Defensoria Publica do Estado do Maranhao
```
# PDF Applet de assinatura digital

O miniaplicativo de assinante de PDF é um desenvolvimento baseado no [assinante de pdf ONTI] (http://cluster.softwarepublico.gob.ar/redmine/projects/firmador-digital). O link está fora do ar, mas havia o projeto originalmente publicado em 2013 com uma licença gratuita.

Funcionalidade:
* Funciona com um token-usb ou keystore de navegador / sistema operacional.
* Permite que você assine um ou vários documentos individuais.
* Valide a validade dos certificados e OCSP (verifique os certificados revogados). Verificação da cadeia de certificados (certificados confiáveis)
* Visualização inline do documento (se permitido pelo navegador). Baixe / abra no sistema operacional.
* Integração simples com aplicativos (exemplos com PHP autônomo e SIU-Toba)


## Integração com aplicativos da Web
* Post `signer.jar`
* Inclua o seguinte código HTML, definindo os valores: 

```
<applet  id="AppletFirmador" code="ar/gob/onti/firmador/view/FirmaApplet"  scriptable="true"  archive="http://url/al/firmador.jar"  width="640"	height="480" >
   <param name="URL_DESCARGA"	 	value="http://url/al/documento.pdf" />
   <param name="MULTIPLE"	 		value="false" />
   <param name="URL_SUBIR"			value="http://url/de/upload" />
   <param name="MOTIVO"  			value="Texto del motivo de la firma" />
   <param name="STAMP_WATERMARK"  	value="false" />
   <param name="CODIGO" 			value="token para control ad-hoc de la sesion" />
   <param name="COOKIE"             value="clave=valor (valor session id de PHP)" />
   <param name="codebase_lookup"    value='false' />
   <param name="classloader_cache" 	value="false" />
</applet>
```

Existem tres tipos via javascript:
```

	/** Callback que se llama al terminar la carga del Applet **/
	function appletLoaded() {
	}
		
	/** Callback que se llama al terminar la firma **/
	function firmaOk() {
	}
		
	/** Para el caso de firma múltiple, agrega un documento al lote de firma **/
	document.AppletFirmador.agregarDocumento(id, url_descarga);
		
	/** Para el caso de firma múltiple, quita un documento previamente agregado al lote de firma **/
	document.AppletFirmador.quitarDocumento(id);
```
#### Integração com SIU-Toba
No caso de usar um projeto Toba pelo menos a versão 2.4, a integração com o componente [ei_firma] já está implementada (https://repositorio.siu.edu.ar/trac/toba/wiki/Referencia/Objetos/ei_firma)

#### Integração com PHP em geral
Para facilitar a integração com aplicativos PHP, uma classe é incluída com alguns utilitários,
É chamado [firmador_pdf.php] (https://github.com/SIU-Toba/firmador/blob/master/sample/www/caso_unico_pdf.php) e é usado na Demonstração do Signatário

## Executando a demonstração
* Publique a pasta sample / www no apache, por exemplo assim: 

```
  ln -s ~/proyectos/firmador/sample/www /var/www/firmador
```
* Abrir en navegador http://localhost/firmador_pdf

## Compilación del Applet
#### Requisitos previos
Las compilaciones se realizaron utilizando las siguientes versiones exactas, probablemente no funcione con versiones más nuevas:
509 / 5000
Resultados de tradução
* Abra no navegador http: // localhost / signer_pdf

## Compilação de miniaplicativo
#### Requisitos anteriores
As compilações foram feitas usando as seguintes versões exatas, provavelmente não funcionará com as versões mais recentes:
* [JAVA JDK1.6.0.33 x86] (http://www.oracle.com/technetwork/java/javase/archive-139210.html)
* [Maven 3.0.4] (http://maven.apache.org/download.html)
* [iText 5.0.2] (http://olex.openlogic.com/packages/itext/5.0.2)
* Instale manualmente o iText 5.0.2 no repositório Maven local 
```
"[Carpeta de Maven]\mvn" install:install-file -Dfile=[Carpeta del IText]\iText-5.0.2.jar -DgroupId=com.lowagie -DartifactId=itext -Dversion=5.0.2 -Dpackaging=jar
```
* Instalar manualmente a biblioteca de plugin de implementação do JDK
```
"[Carpeta de Maven]\mvn" install:install-file -Dfile="[Carpeta de JDK]\jre\lib\plugin.jar" -DgroupId=plugin -DartifactId=plugin -Dversion=1.5 -Dpackaging=jar
```

#### Build
* exportar JAVA_HOME para usar a versao 1.6.:
```
export JAVA_HOME=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
```
* Pela única vez, gere o keystore para autoassinar o mesmo miniaplicativo 
```
keytool -genkey -keyalg rsa -alias key -keystore keystore.jks
#Por ej usar ''password'' como contraseña
```
* Configurar `app/resource/properties/firma.properties` , adicionar ou remover CNs e certificados confiáveis 
```
	...
	IssuerCN4=COMODO Client Authentication and Secure Email CA
	...
	trustedCertificates1=COMODO Client Authentication and Secure
	...
```
* En cada sessão de compilação, executar
```
	export FIRMADOR_STORE=keystore.jks
	export FIRMADOR_ALIAS=key
	export FIRMADOR_STOREPASS=password
	export FIRMADOR_KEYPASS=password
```
* Compilar para 'produção'
```
	mvn -q compile install -DFIRMADOR_PROD -DskipTests=true
```
