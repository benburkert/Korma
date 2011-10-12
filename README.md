# Korma

Delicious SQL for Clojure.

_Korma is just getting started, use at your own peril_

## Usage

```clojure

(use 'korma.db)
(defdb db (postgres {:db "mydb"
                     :user "user"
                     :password "dbpass"}))

(use 'korma.core)
(defentity user)

(select user)
;; executes: SELECT * FROM user

(select user
  (fields :username :id))
;; executes: SELECT username, id FROM user

(select user
  (where {:username "chris"}))
;; executes: SELECT * FROM user WHERE (username = 'chris)'

(select user 
  (where {:active true})
  (order :created)
  (limit 5)
  (offset 3))
;; executes: SELECT * FROM user WHERE (active = TRUE) ORDER BY created DESC LIMIT 5 OFFSET 3

(select user
  (where (or (= :username "chris")
             (= :email "chris@chris.com"))))
;; executes: SELECT * FROM user WHERE (username = 'chris' OR email = 'chris@chris.com')

(select user
  (where {:username [like "chris"]
          :status "active"
          :location [not= nil]))
;; executes SELECT * FROM user WHERE (username LIKE 'chris' AND status = 'active' AND location IS NOT NULL)

(select user
  (where (or {:username "chris"
              :first "chris"}
             {:email [like "*@chris.com"]})))
;; executes: SELECT * FROM user WHERE ((username = 'chris' AND first = 'chris') OR email = 'chris@chris.com)'


(defentity address
 (table-fields :street :city :zip))

(defentity user
 (has-one address))

(select user
 (with address))
;; SELECT address.street, address.city, address.zip FROM user LEFT JOIN address ON user.id = address.user_id

```

## License

Copyright (C) 2011 Chris Granger

Distributed under the Eclipse Public License, the same as Clojure.
