!SLIDE
# <em>O</em>h,  how <em>auth</em>entic. pt <em>II</em>

A gentle intro to open authentication with oauth2

### @softprops

!SLIDE

# Doug

- works for [meetup](http://meetup.com)
- enjoys HTTP
- enjoys [sharing](http://github.com)

!SLIDE

# Oauth2

For the uninitiated

def: a <strong>simple</strong>, redirection-based <strong>authorization</strong> protocol

!SLIDE

# what changed

- trade signing for https requirement
- scopes
- explicit expiry
- response types (
auth code, implicit, user/client crendentials
)

!SLIDE

# current status:

- it's drafty (22 so far)
- not yet official
- plan for changes

!SLIDE

# the gist

1) authorization server

2) token server

!SLIDE

# pregame: 

## register a client w/
 * client_id
 * client_secret
 * redirect_uri

!SLIDE

# A dialog between parties

!SLIDE

# May I... ?

<pre>
curl https://secure.meetup.com/oauth2/authorize
  ?<strong>client_id</strong>=g1b3r1sh
  &<strong>response_type</strong>=code
  &<strong>redirect_uri</strong>=http://your.com/post-auth
  &state=optional-but-recommended
</pre>
!SLIDE

# Yes, you may.

<pre>
http://your.com/post-oauth
  ?<strong>code</strong>=temp-token
  &state=optional-but-recommended
</pre>

!SLIDE

# A token of trust plz

<pre>
curl -X POST https://secure.meetup.com/oauth2/access
  -F '<strong>client_id</strong>=g1b3r1sh'
  -F '<strong>client_secret</strong>=s3cr3tg1b3r1sh'
  -F '<strong>redirect_uri</strong>=http://your.com/post-auth'
  -F '<strong>grant_type</strong>=authorization_code'
  -F '<strong>code</strong>=code_you_just_got'
</pre>
!SLIDE

# Use it wisely

<pre>
{
  <strong>"access_token"</strong>: "k33pm3s3c3r3t",
  "expires_in": 3600,
  "refresh_token": "al30k33pm3s3cr3t"
}
</pre>

!SLIDE

# That's it

<pre>
curl https://api.meetup.com/2/member/self
  ?<strong>access_token=k33pm3s3cf3t</strong>
</pre>
