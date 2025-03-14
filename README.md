## Features
1. apply bus list
2. cancel applied bus list
3. login (nim/nip, password)
4. regis (name, email, nim/nip, number, address, password, password_confirmation)

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
7. CRUD bus entity

## Admin
1. same as co
2. can add/delete co
3. can add/delete user

---

## Database Structure

### User
 id | nim_nip | name | email | phone_number | address | credit_score | password | role
 ---|--------|------|-------|--------|---------|--------------|----------|-----
 PRIMARY KEY, INT | NOT NULL, UNIQUE, VARCHAR| NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, TEXT | INT, DEFAULT = 100, MAX = 100 | NOT NULL, VARCHAR | ENUM (user, co, admin)

### Schedule
 id | bus_schedule | bus_id | route | status
 ---|--------------|--------|-------|--------
 PRIMARY KEY, INT | NOT NULL, DATE | FOREIGN KEY -> busses | NOT NULL, ENUM (sby_gsk, gsk_sby) | NOT NULL, ENUM (pending, done)

### Bus
 id | identity | capacity | available_row | available_col | available_backseat
 ---|----------|----------|---------------|---------------|-------------------
 PRIMARY KEY, INT | NOT NULL, VARCHAR, UNIQUE | NOT NULL, INT | NOT NULL, INT | NOT NULL, INT | NULLABLE, INT

### Seat
 id | bus_id | row_position | col_position | backseat_position | status | user_id
 ---|--------|--------------|--------------|-------------------|--------|-------
 PRIMARY KEY, INT | FOREIGN KEY -> busses | NULLABLE, INT | NULLABLE, INT | NULLABLE, INT | NOT NULL, ENUM (available, booked) | FOREIGN KEY -> users

### schedule_user (pivot)
 id | schedule_id | user_id
 ---|-------------|--------
 PRIMARY KEY, INT | FOREIGN KEY -> schedules | FOREIGN KEY -> users


## STEP
### 1. Authentication
#### Admin :
1. register from start with hardcode
2. can login / logout
3. can regis user and co
4. can delete co

#### Co :
1. only register by admin
2. can login / logout

#### User :
1. can register / login / logout

#### request http
* register : POST api/register
  * request : (name, nim_nip, address, phone_number, email, role, password, password_confirmation)
* login : POST api/login
  * request : (nim_nip, password)
* logout : GET api/logout
  * request : (-)

---

### 2. Bus management
#### Admin :
1. can create/update/delete bus

#### Co :
1. same as admin

#### User :
1. -

#### request http
* create : POST api/bus
  * request : (identity, available_row, available_col, available_backseat)
* update : PUT api/bus/{bus}
  * request : (identity, available_row, available_col, available_backseat)
* delete : DELETE api/bus/{bus}
  * request : (-)
* get all bus : GET api/bus/{bus}
  * request : (-)
* get bus by id : GET api/bus/{bus}
  * request : (-)

#### todo
- [x] admin co can create
- [x] bus identity unique
- [x] admin co can delete
- [x] admin co can update
- [x] bus identity unique when update
- [x] admin co can get all bus
- [ ] admin co can get bus by id
- [ ] user can't access

---

### 3. Schedule Management
#### Admin :
1. can create/update/delete schedule

#### Co :
2. same as admin

#### User :
1. -

#### request http
* create : POST api/schedule
  * request : (bus_schedule, bus_id, route, type)
* update : PUT api/schedule/{schedule}
  * request : (bus_schedule, bus_id, route, type)
* delete : DELETE api/schedule/{schedule}
  * request : (-)
* get all schedules : GET api/schedule
  * request : (-)
* get schedules by id : GET api/schedule/{schedule}
  * request : (-)

---

### 4. Seat Management
#### Admin :
1. can choose seat
2. can see user's seat list
3. can verify or kick user from it seat
4. can change seat position when other seat is empty

#### Co :
1. same as admin

#### User :
1. same as admin without no. 2 & 3

#### request http
* choose seat : POST api/seat
  * request : (bus_id, row_position, col_position, backseat, schedule_id)
* change seat position : PUT api/seat/{seat}
  * request : (bus_id, row_position, col_position, backseat, schedule_id)
* see user's seat list : POST api/seat/schedule
  * request : (schedule_id)
* verify user's seat : PUT api/seat/{seat}
  * request : (-)
* kick user from seat : DELETE api/seat/{seat}
  * request : (-)

---

### 5. Profile Management
#### Admin :
1. can see all users
2. can see all co
3. can see both user and co each profile
4. can delete co or user
5. can update admin profile

#### Co :
1. can update co profile

#### User :
1. same as admin

#### request http
* see all users : GET api/user
  * request : (-)
* see all co : GET api/user/co
  * request : (-)
* see user / co profile : GET api/user/{user}
  * request : (-)
* delete user / co : DELETE api/user/{user}
  * request : (-)
* update profile : PUT api/user/{user}
  * request : (name, email, phone_number, address, role)