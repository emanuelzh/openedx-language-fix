#Actualización de lenguaje en OpenEdx

###Archivos de Configuración

Hacer copia y abrir el archivo
```bash
sudo cp /edx/app/edxapp/edx-platform/lms/envs/common.py /edx/app/edxapp/edx-platform/lms/envs/common.py.bkp
sudo vim /edx/app/edxapp/edx-platform/lms/envs/common.py
```
Actualizar los siguientes parametros

```python
TIME_ZONE = 'America/Mexico_City'
LANGUAGE_CODE = 'es-419'
LANGUAGES = (
   ('es-419', u'Español (Latinoamérica)'),  # Spanish (Latin America)
)
```

Hacer copias de los archivos .json

```bash
cp /edx/app/edxapp/lms.env.json /edx/app/edxapp/lms.env.json.bkp
cp /edx/app/edxapp/cms.env.json /edx/app/edxapp/cms.env.json.bkp
```
Modificar el siguiente bloque de codigo en ambos archivos

```json
"LANGUAGE_CODE": "es-419",
```

###Compilación y reinicio

Las siguientes operaciones deben hacerse dentro de "platform"
```bash
sudo -H -u edxapp bash
source /edx/app/edxapp/edxapp_env
cd /edx/app/edxapp/edx-platform
```

Compilar lenguaje

```bash
paver i18n_generate
```
Actualizar assets
```bash
paver update_assets lms --settings=aws
paver update_assets cms --settings=aws
```

Los siguientes comandos se deben ejecutar fuera del "platform"

```bash
exit
sudo /edx/bin/supervisorctl -c /edx/etc/supervisord.conf restart edxapp:
sudo /edx/bin/supervisorctl -c /edx/etc/supervisord.conf restart edxapp_worker:
```