!SLIDE
<span class="v pad">

# Consuming<br/> <em>Oauth</em> for <em>II</em>

<p>A healthy serving of<br/> open authentication with oauth2</p>

[@softprops](https://twitter.com/softprops/)


!SLIDE
![](/hello/a.png)![](/hello/b.png)![](/hello/c.png)![](/hello/d.png)![](/hello/e.png)

!SLIDE
<span class="v pad">

# Oauth2

For the uninitiated

- a <strong>simple</strong>, 302-based <strong>authorization</strong> protocol
- stop asking for passwords
- [oauth.net/2/](http://oauth.net/2/)

!SLIDE
<span class="v pad">

# change log

## rfc-5849...now

- trade signing for an https requirement
- scopes exactly what do you think <strong>you're</strong> doing?
- profilin' 
auth code, implicit, user/client crendentials


!SLIDE
<span class="v pad">

# current status:

- it's drafty (22 so far)
- not official (yet)
- plan for changes (but hopefully not many)

!SLIDE
<span class="v pad">

# the gist

1) authorization server *

2) token server *

3) choose your profile

\* these can be the same

!SLIDE
<span class="v pad" />

# pregame
## register a client w/

 * client_id
 * client_secret
 * redirect_uri

!SLIDE
<span class="v pad">

# And so begins dialog between parties

!SLIDE

<div class="full">
<h1> act: I</h1>
<h2> Dear Joseph,<br/> I wish to act in your stead</h2>
</div>

!SLIDE
<span class="v pad">

# Q: May I... ?

<pre>
curl https://secure.meetup.com/oauth2/authorize</span>
  ?<strong>client_id</strong>=g1b3r1sh
  &<strong>response_type</strong>=code
  &<strong>redirect_uri</strong>=http://your.com/post-auth
  &state=optional-but-recommended
</pre>

!SLIDE
<span class="v pad">

# Q: May I... ?

<pre>
<span class="un">curl https://secure.meetup.com/oauth2/authorize</span>
  ?<strong>client_id</strong>=g1b3r1sh
  <span class="un">&response_type=code
  &redirect_uri=http://your.com/post-auth
  &state=optional-but-recommended</span>
</pre>

## Who are you anyway?

!SLIDE
<span class="v pad">

# Q: May I... ?

<pre>
<span class="un">curl https://secure.meetup.com/oauth2/authorize
  ?client_id=g1b3r1sh</span>
  &<strong>response_type</strong>=code
  <span class="un">&redirect_uri=http://your.com/post-auth
  &state=optional-but-recommended</span>
</pre>

## Which path do you wish to take?

!SLIDE
<span class="v pad">

# Q: May I... ?

<pre>
<span class="un">curl https://secure.meetup.com/oauth2/authorize
  ?client_id=g1b3r1sh
  &response_type=code</span>
  &<strong>redirect_uri</strong>=http://your.com/post-auth
  <span class="un">&state=optional-but-recommended</span>
</pre>

## Upon that sunny day,<br/> how shall I reachith you?

!SLIDE
<span class="v pad">

# A: Yes, you may.

<pre>
http://your.com/post-oauth
  ?<strong>code</strong>=temp-token
  &state=optional-but-recommended
</pre>


!SLIDE

<div class="full">
  <h1>act: II</h1>
  <h2>obtaining a token of power and trust</h2>
</div>

!SLIDE
<span class="v pad">

# Q: One ticket please?

<pre>
curl -X POST https://secure.meetup.com/oauth2/access
  -F '<strong>client_id</strong>=g1b3r1sh'
  -F '<strong>client_secret</strong>=s3cr3tg1b3r1sh'
  -F '<strong>redirect_uri</strong>=http://your.com/post-auth'
  -F '<strong>grant_type</strong>=authorization_code'
  -F '<strong>code</strong>=code_you_just_got'
</pre>


!SLIDE
<span class="v pad">

# Q: One ticket please?

<pre>
<span class="un">curl -X POST https://secure.meetup.com/oauth2/access</span>
  -F '<strong>client_id</strong>=g1b3r1sh'
  -F '<strong>client_secret</strong>=s3cr3tg1b3r1sh'
  <span class="un">-F 'redirect_uri=http://your.com/post-auth'
  -F 'grant_type=authorization_code'
  -F 'code=code_you_just_got'</span>
</pre>


!SLIDE
<span class="v pad">

# Q: One ticket please?

<pre>
<span class="un">curl -X POST https://secure.meetup.com/oauth2/access
  -F 'client_id=g1b3r1sh'
  -F 'client_secret=s3cr3tg1b3r1sh'</span>
  -F '<strong>redirect_uri</strong>=http://your.com/post-auth'
  <span class="un">-F 'grant_type=authorization_code'
  -F 'code=code_you_just_got'</span>
</pre>


!SLIDE
<span class="v pad">

# Q: One ticket please?

<pre>
<span class="un">curl -X POST https://secure.meetup.com/oauth2/access
  -F 'client_id=g1b3r1sh'
  -F 'client_secret=s3cr3tg1b3r1sh'
  -F 'redirect_uri=http://your.com/post-auth'</span>
  -F '<strong>grant_type</strong>=authorization_code'
  -F '<strong>code</strong>=code_you_just_got'
</pre>

!SLIDE
<span class="v pad">

# A: Enjoy the show

<pre>
{
  <strong>"access_token"</strong>: "k33pm3s3c3r3t",
  "expires_in": 3600,
  "refresh_token": "al30k33pm3s3cr3t"
}
</pre>

!SLIDE
<span class="v pad">

# That's it

<pre>
curl https://api.meetup.com/2/member/self
  ?<strong>access_token=k33pm3s3cf3t</strong>
</pre>

!SLIDE
<span class="v pad">

# Well, almost...

- users can forsake (revoke) you
- the clock could strike midnight (expires_in)
- air ball (she's beyond you scope of access)

!SLIDE
<span class="pad">

# Plan for a bumpy ride

<pre>
from urllib2 import (Request, openurl, urlencode,
                     HttpError)
def get(path, params = {}):
  try:
    parse(openurl(Request("%s%s?%s" % (
      host, path,
      urlencode(with_access(params)))).read())
  except HTTPError, e:
     <span class="un"># dirty jobs</span>
</pre>

!SLIDE
<span class="pad">

<pre>
except HTTP, e:
  if(e.code == 401): <span class="un"># denied</span>
    try:
      <span class="un"># freshen up</span>
      reset(parse(openurl(Request(<strong>TOKEN_URI</strong>, data = {
        <strong>'client_id'</strong>:'g1b3r1sh'
        <strong>'client_secret'</strong>:'s3cr3tg1b3r1sh'
        <strong>'grant_type'</strong>:'refresh_token',
        <strong>'refresh_token'</strong>:'al30k33pm3s3cr3t'
       })).read()))
       return get(path, params)
    except HttpError, e2: <span class="un"># revoked</span>
       raise WhiteFlag
</pre>

!SLIDE
<span class="v pad">

# {

'code-demo': intermission

# }
