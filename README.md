#MySQL code generator

##Installation

	go build pg-golang-generator.go

Usage

	pg-golang-generator conn_string tablename

	pg-golang-generator "username=postgres user=postgres sslmode=disable" > generated.code

example of generate:

	type Titanic struct {
			id int64
			passenger_id int64
			pclass int64
			name string
			sex string
			age float64
			sibsp int64
			parch int64
			ticket string
			fare float64
			cabin string
			embarked string
			res bool
	}
		func getTitanic(db *sql.DB,  ) Titanic {
			var(
				ret Titanic
				id sql.NullInt64
				passenger_id sql.NullInt64
				pclass sql.NullInt64
				name sql.NullString
				sex sql.NullString
				age sql.NullFloat64
				sibsp sql.NullInt64
				parch sql.NullInt64
				ticket sql.NullString
				fare sql.NullFloat64
				cabin sql.NullString
				embarked sql.NullString
				res sql.NullBool
			)
			sql_s := " SELECT id,passenger_id,pclass,name,sex,age,sibsp,parch,ticket,fare,cabin,embarked,res FROM titanic WHERE =? "
			rows, err := db.Query(sql_s, )
	 		errorCheck(err)
			defer rows.Close()
			for rows.Next() {
				err = rows.Scan(&id, &passenger_id, &pclass, &name, &sex, &age, &sibsp, &parch, &ticket, &fare, &cabin, &embarked, &res)
				errorCheck(err)
				ret.id=sql2Int(id)
				ret.passenger_id=sql2Int(passenger_id)
				ret.pclass=sql2Int(pclass)
				ret.name=sql2String(name)
				ret.sex=sql2String(sex)
				ret.age=sql2Float(age)
				ret.sibsp=sql2Int(sibsp)
				ret.parch=sql2Int(parch)
				ret.ticket=sql2String(ticket)
				ret.fare=sql2Float(fare)
				ret.cabin=sql2String(cabin)
				ret.embarked=sql2String(embarked)
				ret.res=sql2Bool(res)
			}
			return ret
		}

include function:
	
	func sql2String(str sql.NullString) string {
		if str.Valid {
			return str.String
		}
		return ""
	}

	func sql2Int(str sql.NullInt64) int64 {
		if str.Valid {
			return str.Int64
		}
		return 0
	}

	func sql2Float(str sql.NullFloat64) float64 {
		if str.Valid {
			return str.Float64
		}
		return 0.0
	}

	func sql2Bool(str sql.NullBool) bool {
		if str.Valid {
			return str.Bool
		}
		return false
	}

	func errorCheck(err error) {
		if err != nil {
			panic(err.Error())
		}
	}


