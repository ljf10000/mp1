# object define

TIME-STAMP: int
UID: int // user id
GID: int // group id
SID: int // student id

OBJ-ERROR:
	errno 		int
	errstr		string

OBJ-USER-IDENTIFY:
	uid 		int
	session 	string

OBJ-GROUP-IDENTIFY:
	gid 		int
	opengid 	string

OBJ-SECRET:
	encryptedData 	string
	iv				string

OBJ-LEASE:
	begin		TIME-STAMP
	end			TIME-STAMP

OBJ-GROUP-INFO:
	gid			int
	opengid		string
	adviser 	UID
	payer 		UID
	lease 		OBJ-LEASE
	create 		TIME-STAMP
	students 	MAP-STUDENT
	teachers 	MAP-TEACHER
	patriarchs	MAP-PATRIARCHS

OBJ-CHECKIN-STUDENT:
	name		string
	relation 	string
	sex 		ENUM-SEX

MAP-STUDENT:
	k: SID(string)
	v: OBJ-STUDENT

MAP-TEACHER:
	k: UID(string)
	v: OBJ-TEACHER

MAP-PATRIARCH:
	k: UID(string)
	v: OBJ-PATRIARCH

OBJ-STUDENT：
	name		string
	sex 		ENUM-SEX
	relation 	MAP-STUDEN-RELATION

MAP-STUDEN-RELATION:
	k: UID(string)
	v: "RELATION"

OBJ-TEACHER:
	name		string
	nick 		string

OBJ-PATRIARCH:
	name 		string
	nick		string
	relation	MAP-PATRIARCH-RELATION

MAP-PATRIARCH-RELATION:
	k: SID(string)
	v: "RELATION"

OBJ-TOPIC:
	uid 		UID // topic creater's UID
	create 		TIME-STAMP
	type 		int // topic type
	key 		string
	value 		string

OBJ-TOPIC-LOG:
	uid 		UID // topic log creater's UID
	create 		TIME-STAMP
	topic		string // topic key
	key 		string
	value 		string

OBJ-PAY:
	timeStamp	string
	nonceStr 	string
	package		string
	signType 	string
	paySign		string

ENUM-SEX: int
	0: unknow
	1: man
	2: woman

start:
	if get sharetick then
		// goto: start from group
	else
		// goto: start normal
	end if


start from group:
	if get uid then
		if session NOT timeout then
			request url: user/gsecret
		else // session timeout
			request url: user/login-gsecret
		end if
	else // no uid
		request url: rand/login-gsecret
	end if


start normal:
	if get uid then
		if session NOT timeout then
			do nothing
		else // session timeout
			request url: user/login
		end if
	else // no uid
		request url: rand/login
	end if

pay:
	request url: group/pay
