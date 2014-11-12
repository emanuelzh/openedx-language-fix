#Setup language on OpenEdx

###General considerations

This guide should work fine on fresh production installs, both Vagrant and Ubuntu 12.

We experienced some issues related to database on the Ubuntu install but we solve it following [this instructions](https://groups.google.com/forum/#!topic/edx-code/GrsDfnILgE8).

All the language files are already on the [openedx-platform repository](https://github.com/edx/edx-platform/tree/master/conf/locale).

###Updating configuration files

Make a copy and edit
```bash
sudo cp /edx/app/edxapp/edx-platform/lms/envs/common.py /edx/app/edxapp/edx-platform/lms/envs/common.py.bkp
sudo vim /edx/app/edxapp/edx-platform/lms/envs/common.py
```
Locate the following block and update with the language settings you want

```python
TIME_ZONE = 'America/Mexico_City'
LANGUAGE_CODE = 'es-419'
LANGUAGES = (
   ('es-419', u'Español (Latinoamérica)'),  # Spanish (Latin America)
)
```

Make backups of the following .json files

```bash
cp /edx/app/edxapp/lms.env.json /edx/app/edxapp/lms.env.json.bkp
cp /edx/app/edxapp/cms.env.json /edx/app/edxapp/cms.env.json.bkp
```
Edit the following block on the two files

```json
"LANGUAGE_CODE": "es-419",
```
###Compiling, compiling and restarting

The following operations must be perform under "platform"
```bash
sudo -H -u edxapp bash
source /edx/app/edxapp/edxapp_env
cd /edx/app/edxapp/edx-platform
```

Compile languages files. This part failed a dozen times with old (>10 days old) installations. In some point something went wrong and this command din't work, so, keep in mind that you have to update the platform to make this works

```bash
paver i18n_generate
```
Update assets
```bash
paver update_assets lms --settings=aws
paver update_assets cms --settings=aws
```

Next commands have to be performed outside the "platform"

```bash
exit
sudo /edx/bin/supervisorctl -c /edx/etc/supervisord.conf restart edxapp:
sudo /edx/bin/supervisorctl -c /edx/etc/supervisord.conf restart edxapp_worker:
```