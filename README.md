## Features
1. next bus schedule
2. apply bus list
3. cancel applied bus list
4. login (nim/nip, password)
5. regis (name, email, nim/nip, number, address, password, password_confirmation)

## User
1. have max 100 credit score
2. if credit score > 90 allowed to apply bus list
3. if credit score < 100, score increase by 1 everyday
4. if applied to bus list but not verified (applied but not present in real time) score decrease by 5

## Co
1. can see all user include their profiles
2. can manipulate bus list (add or canceling user from bus list)
3. deleting bus list
4. verify user's present
5. update bus schedule
6. create bus schedule

---

## Database Structure

### User
 id | nim_nip | name | email | number | address | credit_score | password | role
 ---|--------|------|-------|--------|---------|--------------|----------|-----
 PRIMARY KEY, INT | NOT NULL, UNIQUE, VARCHAR| NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, TEXT | INT, DEFAULT = 100, MAX = 100 | NOT NULL, VARCHAR | ENUM (user, Co, admin)

### Schedule
 id | bus_schedule | number | capacity | ikut_berangkat | ikut_pulang | berangkat | pulang | pp | pp_khusus
 ---|--------------|--------|----------|----------------|-------------|----------|--------|----|----------
 PRIMARY KEY, INT | NOT NULL, DATE | NOT NULL, VARHCAR | NOT NULL, INT | INT | INT | INT | INT | INT | INT

### schedule_user (pivot)
id | user_id | schedule_id | type | status
---|---------|-------------|------|-------
PRIMARY KEY, INT | FOREIGN KEY -> users | FOREIGN KEY -> schedules | ENUM (ikut_berangkat, ikut_pulang, berangkat, pulang, pp, pp_khusus) | ENUM (applied, verified)


## STEP
### 1. Auth Section
#### User / Co :
1. can register
2. can login/logout
3. can edit profile

#### request :
* register :
   * (name, email, nim/nip, number, address, password, role , password_confirmation)
* login/logout :
  * (nim/nip, password) 
* update profile :
  * (name, email, number, address), param (user_id)

---

### 2. Bus List
#### User :
1. can apply bus list
2. can cancel bus list

#### request :
* apply bus list :
  * (user_id, schedule_id)
* cancel bus list :
  * (user_id, schedule_id)

#### Co :
1. can delete bus list
2. can verify user's present
3. can update bus schedule
4. can create new bus schedule
5. can apply / cancel another user
6. can see all user's profiles

#### request :
* delete bus list :
  * params (schedule_id)
* verify user's present :
  * (user_id), param (schedule_id)
* update bus schedule :
  * (bus_schedule, capacity, nummber, ikut_berangkat, ikut_pulang, berangkat, pulang, pp, pp_khusus), param (schedule_id)
* create new bus schedule :
  * (bus_schedule, capacity, number)
* apply/cancel another user
  * (user_id, schedule_id)
* see user's profile
  * param (user_id)