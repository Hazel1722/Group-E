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

__________________________________________________________________
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
    was_published_recently.admin_order_field = 'pub_date'
    was_published_recently.boolean = True
    was_published_recently.short_description = 'Published recently?'

class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __unicode__(self):  # Python 3: def __str__(self):
        return self.choice_text

from django.contrib import admin
from polls.models import Poll
from polls.models import Choice

admin.site.register(Choice)

#admin.site.register(Poll)
#class PollAdmin(admin.ModelAdmin):
    #fields = ['pub_date', 'question']
     #fieldsets = [
        #(None,               {'fields': ['question']}),
        #('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    #]
     
    

#admin.site.register(Poll, PollAdmin)
class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 3

class PollAdmin(admin.ModelAdmin):
    fieldsets = [
        (None,               {'fields': ['question']}),
        ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]
    list_display = ('question', 'pub_date')
    list_display = ('question', 'pub_date', 'was_published_recently')
    list_filter = ['pub_date']
    search_fields = ['question']
    date_hierarchy = 'pub_date'
admin.site.register(Poll, PollAdmin)


