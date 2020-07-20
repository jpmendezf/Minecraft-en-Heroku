# Spigot en Heroku

[![Implementar en Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## Ejecute su servidor Minecraft Spigot en la nube con Heroku.

Este es un Heroku [buildpack] (https://devcenter.heroku.com/articles/buildpacks), para instalarlo necesita instalar Heroku [toolbelt] (https://toolbelt.heroku.com).

## Requisito

1. El toolbelt de herramientas Heroku instalado

2. Una cuenta gratuita de Heroku. [Regístrese aquí] (https://signup.heroku.com)

3. Una cuenta ngrok (para tunelización de ip). [Regístrese aquí] (https://ngrok.com/signup)

4. Una cuenta de Dropbox para la sincronización de archivos. [Regístrese aquí] (https://www.dropbox.com/login) (información a continuación)

## Uso

Después de descargar e instalar la CLI de Heroku, abra el símbolo del sistema (cmd) y escriba

```
heroku login
```
con sus credenciales de cuenta.

Luego, simplemente copie las siguientes líneas a su Símbolo del sistema (cmd)

```
heroku create
heroku buildpacks:add heroku/jvm
heroku buildpacks:add https://github.com/jpmendezf/Minecraft-en-Heroku
heroku ps:exec
git commit -m "Heroku Exec" --allow-empty
done
```
Luego, agregue manualmente estos comandos con su token de autenticación Ngrok, enlace del archivo de configuración de la aplicación de Dropbox (lea a continuación)

```
$ heroku config:set NGROK_API_TOKEN="xxxxx"
$ heroku config:set DBCONFIG="xxxxx"
```
Luego copie estos comandos en el cmd

```
git push heroku master
heroku ps:scale worker=1
done
```

¡Y listo!

Vaya [aquí] (https://dashboard.ngrok.com/status/) para verificar la ip de su servidor.

## Sincronización de archivos

El sistema de archivos Heroku es efímero, lo que significa que los archivos escritos en el sistema de archivos se destruirán cuando se reinicie el servidor.

Minecraft guarda todos los datos del servidor en archivos planos en el sistema de archivos. Por lo tanto, si desea mantener su mundo, deberá sincronizarlo con Dropbox o AWS S3.

De forma predeterminada, la sincronización de Dropbox respaldará automáticamente sus datos cada 5 minutos.

Puede agregar Amazon S3 para sincronizar sus datos

Primero, cree una cuenta de AWS y un depósito S3. Luego configure el depósito y sus claves de AWS de esta manera:

```
$ heroku config:set AWS_BUCKET=your-bucket-name
$ heroku config:set AWS_ACCESS_KEY=xxx
$ heroku config:set AWS_SECRET_KEY=xxx
```

El buildpack sincronizará su mundo con el bucket cada 60 segundos, pero esto es configurable configurando la variable de configuración AWS_SYNC_INTERVAL.

## Personalizando Minecraft

Puedes elegir la versión de Minecraft configurando MINECRAFT_VERSION de esta manera:

Por defecto, la versión es `1.8.8-R0.1-SNAPSHOT-latest`, el nombre de los archivos se encuentra en el sitio oficial [GetBukkit](https://getbukkit.org/spigot).

```
heroku config:set MINECRAFT_VERSION="1.8.8-R0.1-SNAPSHOT-latest"
```
También puede configurar las propiedades del servidor creando un archivo server.properties en su proyecto y agregándolo a Git. Así es como establecería cosas como el modo creativo y la dificultad Hardcore. Las diversas opciones disponibles se describen en Minecraft Wiki.

Puede agregar archivos como `` banned-players.json``, `` banned-ips.json``, `` ops.json``, `` whitelist.json`` a su repositorio de Git y el servidor de Minecraft recogelos.

## Consejos

Puede crear su propio servidor en su computadora con sus propios complementos, mundos y luego comprimirlo en un archivo con el nombre ** backup.zip **. Súbelo a Dropbox y tendrá su propio servidor configurado.