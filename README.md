### Install Passport Js ###

### NPM
```javascript
npm install passport passport-local passport-local-mongoose
```
#### App.js
```javascript
const passport = require('passport');
const expressSession = require('express-session');

//just below of the view engine
app.use(expressSession({
  resave:false, 
  saveUninitialized:false,
  secret:"jay Hind Bharat mata ki jay"
}));

app.use(passport.initialize()); 
app.use(passport.session()); 
passport.serializeUser(usersRouter.serializeUser());
passport.deserializeUser(usersRouter.deserializeUser());

```
#### index.js
```javascript
const passport = require('passport'); 
const localStategy = require('passport-local');
passport.use(new localStategy(userModel.authenticate()));
```
For User Registration 
```javascript
router.post('/register', function(req,res){
  var userData = new userModel({
    username:req.body.username, 
    profileImage:req.body.profileImage,
  })

  userModel.register(userData,req.body.password).then(function(registerUser){
    passport.authenticate('local')(req,res,function(){
      res.redirect('puth here path.');
    })
  })
})
```
For Login Verification 
```javascript
router.post('/login',passport.authenticate('local',{
  successRedirect:'feed',
  failureRedirect:'/login'
}))
```
isLoggedIn middleware
```javascript
function isLoggedIn(req,res,next){
  if(req.isAuthenticated()){
    return next();
  }
  res.redirect('/login');
}
```
Logout 
```javascript
router.get('/logout',function(req,res,next){
  req.logout(function(err){
    if(err) return next(err); 
    res.redirect("/");
  })
})
```
### In Schema 
```javascript
const mongoose = require('mongoose'); 
const plm = require('passport-local-mongoose')

mongoose.connect('mongodb://0.0.0.0/passport'); 

const userShema = mongoose.Schema({
  username : String, 
  profileImg : String, 
  password : String
})

userShema.plugin(plm)
module.exports = mongoose.model("user",userShema);
```
