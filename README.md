Group-E
=======

Group Activities (2013)

leader=Mickeevinth Singcol
member=Los Bañes Hazel
member=Labrador John James
member=Cabije Mary Grace

_______________________________________________________________________________________________________
from django.db import models

import datetime
from django.utils import timezone

# Create your models here.
class Poll(models.Model):
    question = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    def __unicode__(self):  # Python 3: def __str__(self):
        return self.question
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __unicode__(self):  # Python 3: def __str__(self):
        return self.choice_text

from django.contrib import admin
from polls.models import Poll

#admin.site.register(Poll)

class PollAdmin(admin.ModelAdmin):
    #fields = ['pub_date', 'question']
     fieldsets = [
        (None,               {'fields': ['question']}),
        ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
     
    

admin.site.register(Poll, PollAdmin)

