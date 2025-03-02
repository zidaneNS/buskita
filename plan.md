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
 nim_nip | name | email | number | address | credit_score | password | role
 --------|------|-------|--------|---------|--------------|----------|-----
 PRIMARY KEY, VARCHAR | NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, VARCHAR | NOT NULL, TEXT | INT, DEFAULT = 100, MAX = 100 | NOT NULL, VARCHAR | ENUM (user, Co, admin)

### List
 id | bus_schedule | number | capacity | ikut_berangkat | ikut_pulang | berangkat | pulang | pp | pp_khusus
 ---|--------------|--------|----------|----------------|-------------|----------|--------|----|----------
 PRIMARY KEY, INT | NOT NULL, DATE | NOT NULL, VARHCAR | NOT NULL, INT | INT | INT | INT | INT | INT | INT

### user_list (pivot)
id | user_id | list_id | type
---|---------|---------|-----
PRIMARY KEY, INT | FOREIGN KEY -> users | FOREIGN KEY -> lists | ENUM (ikut_berangkat, ikut_pulang, berangkat, pulang, pp, pp_khusus)


## STEP
### 1. Auth Section
#### User / Co :
1. can register
2. can login
3. can edit profile

### 2. Bus List
#### User :
1. can apply bus list
2. can cancel bus list

#### Co :
1. can delete bus list
2. can verify user's present
3. can update bus schedule
4. can create new bus schedule
5. can apply / cancel another user
6. can see all user's profiles