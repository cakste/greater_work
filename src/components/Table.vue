<template>
	<div class="table">
	<h1 v-if="!filledData">Loading data...</h1>
	<table id="myTable1">
		<thead>
			<tr>
				<!---Setting the table headers to the key values dynamically.
					Interchanging underscore for space in table as well. -->
				<th v-for="(col, i) in columns" :key="i"
				v-on:click="sortData(col)">{{ col.replaceAll('_', ' ') }}</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="(user, i) in getRows()" :key="i">
				<!-- Instead of iterating over the keys in the user.
				This doesn't require user data to always have the same order
				of key-value pairs. Accessing each value by the key instead.-->
				<td v-for="(col, i) in columns" :key="i">
					{{ user[col] }}
				</td>
				<!---
				<td v-for="(val, j) in user" :key="j">{{ val }}</td>
				<td>{{user.$(id)}}</td>
				-->
			</tr>
		</tbody>
	</table>
	<div class="pagination">
		<div class="number"
		v-for="i in numPages()" :key="i"
		v-bind:class="[i == currentPage ? 'active' : '']"
		v-on:click="changePage(i)">{{i}}
		</div>
	</div>
  </div>
</template>



<script>
/* Todo
	* Create a refresh button?


*/

export default {
	name: 'HelloWorld2',
	props: {
		msg2: String,
	},

	data () {
		return {
			users: null,
			filledData: null,
			columns: null,
			usersPerPage: 12,
			currentPage: 1,
			ascending: true,
			sortCol: null,
			message: null,
			maxColumns: 5,
		}
	},

	async created () {
		function sleep(ms) {
			return new Promise(resolve => setTimeout(resolve, ms));
		}

		const data_fetched = false
		//Fetch the data upon creation of the component.
		while (data_fetched === false) {
			try {
				const response = await fetch("http://dummy.restapiexample.com/api/v1/employees")
				const response_json = await response.json()

				this.users = response_json.data
				this.message = response_json.message
				console.log(response_json)
				break;
			} catch (e) {
				let sleep_time = 300//Math.floor(Math.random()*400+100)
				await sleep(sleep_time)
				// Some type of sleep function? Not really sure how this too many requests occur.
				continue;
			}
		}

		await this.fillOutData();

		//this.users[5]["employee_name"] = ''

		//this.columns = this.updateColumns()

	},

	computed: {

	},

	methods: {
		fillOutData () {
			/* We should not assume key-value pairs for the users are perfect.

			Some implementation decisions:
				*	The empty string is regarded as the zero value.
				*	If one of the users has a key with a non zero value a column will
			 		be displayed for that key.
				*	If a user does not have a value or a zero value for a key
					which another user has a value for, the empty string will be displayed for that user.

			A consequence of these rules is that keys for which all users has the
			zero value will be disregarded. As for the 'profile_image' data when this
			was created.
			*/

			let keyValues = []
			var valueCounts = {}
			this.filledData = this.users

			// Include all keys that some user has a value for.
			for (let i = 0; i < this.filledData.length; i++) {
				for (let key in this.filledData[i]) {
					if (this.filledData[i][key] !== "") {
						if (!keyValues.includes(key)) {
							keyValues.push(key)
							valueCounts[key] = 1
						} else {
							valueCounts[key]++
						}

					}

					/*
					if (!keyValues.includes(key) && this.filledData[i][key] !== "") {
						keyValues.push(key)
					}
					*/
				}
			}
			console.log(valueCounts)

			/*
				Take out maxColumns number of keys which has the most non-zero values
				amongst all users.
			*/
			if (keyValues.length > this.maxColumns) {
				var items = Object.keys(valueCounts).map(
					(key) => {return [key, valueCounts[key]]});
				items.sort(
					(first, second) => { return -(first[1] - second[1]) }
				);
				keyValues = items.map(
					(e) => {return e[0]}
				);
				console.log(keyValues)
				keyValues = keyValues.slice(0, this.maxColumns)
			}


			// If there is a key that some user has a non-zero value,
			// set zero values for that key for all other users.
			for (let i = 0; i < this.filledData.length; i++) {
				for (let j = 0; j < keyValues.length; j++) {
					if (!(keyValues[j] in this.filledData[i])) {
						this.filledData[i][keyValues[j]] = '';
					}
				}
			}

			// The columns that are to be displayed correspond to the keyValues
			// chosen here.
			this.columns = keyValues
		},
		/*
		updateColumns () {
			//Gets columns based on our computed value.
			const columns = []

			if (this.filledData !== null) {
				// Should try if an empty list crashes this!
				for (let key in this.filledData[0]) {
					columns.push(key)
				}
			} else {
				console.log("Return null in getColumns")
				return null
			}
			console.log("Returning columns")
			console.log(columns)
			return columns

		},
		*/
		sortData (col) {
			// Keep track of the last sorted column. Also keep track of if
			// the data is sorted in a ascending or descending way. If the same
			// column is sorted again, it is sorted in the other way.

			if (this.sortCol === col) {
				this.ascending = !this.ascending
			} else {
				this.ascending = true
				this.sortCol = col
			}

			var ascending = this.ascending
			this.filledData.sort(function(a,b) {
				// Always shove zero values towards the bottom in sorting.
				// Both for ascending and descending sorting.
				if (a[col] === '' || b[col] === '') {
					if (a[col] === '' && b[col] === '') {
						return 0
					}
					if (a[col] === '' && b[col] !== '') {
						return 1
					} else {
						return -1
					}
				}
				// Sorting.
				if (a[col] > b[col]) {
					return ascending ? 1 : -1
				} else if (a[col] < b[col]) {
					return ascending ? -1 : 1
				}
				return 0
			})
			console.log(col)
		},
		getRows () {
			if (this.filledData === null) {
				return null
			}
			var start = (this.currentPage - 1) * this.usersPerPage
			var end = start + this.usersPerPage
			return this.filledData.slice(start, end)

		},
		numPages () {
			if (this.filledData === null) {
				return 0
			}
			return Math.ceil(this.filledData.length / this.usersPerPage)
		},
		changePage (pageNum) {
			this.currentPage = pageNum
		}
	}
}

</script>

<style scoped>
table {
  font-family: Poppins, sans-serif;
  width: 750px;
  border-collapse: collapse;
  border: 5px solid #1cba93;
  margin: 40px 10px 0 10px;
  margin-left: auto;
  margin-right: auto;
  box-shadow:
  0px 0px 20px rgba(0,0,0,0.10),
  0px 10px 20px rgba(0,0,0,0.05),
  0px 20px 20px rgba(0,0,0,0.05),
  0px 30px 20px rgba(0,0,0,0.05);

}


table th {
  text-transform: uppercase;
  text-align: left;
  background: #1cba93;
  color: #FFF;
  cursor: pointer;
  padding: 8px;
  min-width: 30px;
}
table th:hover {
        background: #6fcbb5;
      }
table td {
  text-align: left;
  padding: 8px;
  border-right: 2px solid #7D82A8;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #f1eff7;
}

.pagination {
  font-family: poppins, sans-serif;
  text-align: right;
  width: 750px;
  padding: 8px;
  margin-left: auto;
  margin-right: auto;
}

.arrow {
  float: right;
  width: 12px;
  height: 15px;
  background-repeat: no-repeat;
  background-size: contain;
  background-position-y: bottom;
}

.number {
  display: inline-block;
  padding: 4px 10px;
  color: #FFF;
  border-radius: 4px;
  background: #1cba93;
  margin: 0px 5px;
  cursor: pointer;
}
.number:hover, .number.active {
  background: #6fcbb5;
}


</style>
