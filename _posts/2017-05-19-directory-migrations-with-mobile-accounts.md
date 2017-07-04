---
layout: post
title: "Directory Migrations with mobile accounts"
author: "Geoff"
---
Unbind the Macs from the existing domain:

{% highlight text %}
search_path="$(/usr/bin/dscl /Search read . CSPSearchPath | awk '/Active Directory/{print substr($0,2)}')"
/usr/sbin/dsconfigad -remove -force -u none -p none
/usr/bin/dscl /Search/Contacts delete . CSPSearchPath "$search_path"
/usr/bin/dscl /Search delete . CSPSearchPath "$search_path"
/usr/bin/dscl /Search change . SearchPolicy dsAttrTypeStandard:CSPSearchPath dsAttrTypeStandard:NSPSearchPath
/usr/bin/dscl /Search/Contacts change . SearchPolicy dsAttrTypeStandard:CSPSearchPath dsAttrTypeStandard:NSPSearchPath
{% endhighlight %}

Remove cached mobile account plists. Mobile account UIDs tend to begin in the 1000s for Open Directory and much higher for Active Directory, so it makes it easy to isolate only mobile accounts:

{% highlight text %}
while IFS= read -r user ; do
   rm -f /var/db/dslocal/nodes/Default/users/"$user".plist
done < <(/usr/bin/dscl . list /users uid | awk '$2>=1000{print $1}')
{% endhighlight %}

Remove the sqlindex files from dslocal. These files cache the account plists in dslocal, and they will be rebuilt automatically. The computer will need to be rebooted in order for the caches to clear.

{% highlight text %}
rm -f /var/db/dslocal/nodes/Default/sqlindex*
shutdown -r now
{% endhighlight %}

Bind the Mac to Open Directory upon reboot with your Directory Binding config. You may need to experiment with this step as I do not recall whether or not you'll need to either kill directory services before the account plists are regenerated using the new directory attributes. Worse comes to worse, include a restart in the bind policy.

Change ownership for each mobile account's home directory with their new UID assigned from Open Directory. Assuming your users' `NFSHomeDirectory` path is the default, then the following will work:

{% highlight text %}
for homedir in /Users/* ; do
   user="$(basename "$homedir")"
   if [[ "$user" != "Shared" && "$user" != "Guest" ]] ; then
      user_uid="$(/usr/bin/dscl . read /users/"$user" UniqueID | awk -F': ' '{print $NF}')"
      chown -R "$user_uid" "$homedir"
   fi
done
{% endhighlight %}
