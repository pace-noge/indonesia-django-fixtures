# Wilayah Administrasi Indonesia

**Cara penggunaan**

Data in digunakan untuk app bernama 'geo', silahkan app in di generate dengan cara ```./manage.py startapp geo)```.
Buat folder fixtures di dalam app `geo`, copy atau pindahkan file provinces.json, regency.json, district.json, dan village.json ke dalam folder fixtures.

contoh format models:
```python
from django.db import models


class Province(models.Model):
    name = models.CharField(max_length=120, unique=True)

    def __unicode__(self):
        return self.name


class Regency(models.Model):
    province = models.ForeignKey(Province, on_delete=models.CASCADE)
    name = models.CharField(max_length=120)

    class Meta(object):
        unique_together = (('name', 'province'))

    def __unicode__(self):
        return u'%s, %s' % (self.name, self.province.name)


class District(models.Model):
    regency = models.ForeignKey(Regency, on_delete=models.CASCADE)
    name = models.CharField(max_length=120)

    class Meta(object):
        unique_together = (('name', 'regency'))

    def __unicode__(self):
        return u'%s, %s' % (self.name, self.regency.name)


class Village(models.Model):
    district = models.ForeignKey(District, on_delete=models.CASCADE)
    name = models.CharField(max_length=120)


    def __unicode__(self):
        return u'%s, %s' % (self.name, self.district.name)

```

untuk update database jangan lupa makemigrations, dan migrate
```bash
./manage.py makemigrations
./manage.py migrate
./manage.py loaddata province
./manage.py loaddata regency
./manage.py loaddata district
./manage.py loaddata village
```
