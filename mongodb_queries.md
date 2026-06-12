
MongoDB Queries

1.	Question 1: Find all flight records where the country is exactly "Spain". (Use the Filter 
bar)

db.flights.find({
  country:"Spain"
})


2.	Question 2: Find all flight records that happened in the year 2020 AND during the month_name "MAY". (Use the Filter bar)



db.flights.find({
  year: 2020 ,month_name : "MAY"
  
})


3.	Question 3: Find all flight records where the total_flights count is strictly greater than 500. (Use the Filter bar)




db.flights.find({ 
  total_flights : { $gt : 500}
  
})

4.	Question 4: Find all flight records for these three specific airports: "Porto", "Kaunas", and "Treviso". 


(Use the Filter bar)

db.flights.find({ 
  airport_name : { 
    $in  : ["Porto", "Kaunas", "Treviso"]
    }
  
})


5.	Question 5: Sort the entire dataset to show the highest total_flights at the top, and limit the results to show only the top 5 busiest records. (Use the Aggregations tab)



db.flights.aggregate([


   {$sort :{total_flights : -1}

},   {$limit : 5}

])


6.	Question 6: Reshape the output documents to display only airport_code and flight_date, hide the default _id field, and rename the total_flights field to "traffic_volume". (Use the Aggregations tab)

db.flights.aggregate([{
   $project : {
              _id: 0 , 
              airport_code : 1 ,
              flight_date : 1, 
              total_flights : {'total_volume': 1}
   } 
   }
   
   ])


7.	Question 7: Calculate the total overall sum of all departures and all arrivals added together across the entire dataset. (Use the Aggregations tab)

db.flights.aggregate([
  {
    $group : {_id:null,
             departs:{$sum:'$departures'},
             arrivals:{$sum : '$arrivals'}}
  },{
    $project : {_id:0,departs : 1,arrivals: 1,
                overall_sum : {$add:['$departs','$arrivals']},}
  
  }
  
  ])


8.	Question 8: Count how many total flight record documents exist in the dataset for each individual country. (Use the Aggregations tab)



db.flights.aggregate([{
  $group : {_id : '$country',
            tot_flight_records : {$sum :'$total_flights'}
}

}

])


9.	Question 9: Filter for the year 2016, calculate the average total_flights for each country, and then show only the countries where that average is greater than 100. (Use the Aggregations tab)




db.flights.aggregate([
  {
    $group : {
     _id : '$country', 
     avg_flights: {$avg :'$total_flights'} 
}
  },
  {
    $match :{avg_flights:{$gt : 100}}

}

])


10.	Question 10: Group the total flight volumes by both year and month_number at the same time, and sort the final results in chronological order (by year and month). (Use the Aggregations tab)




db.flights.aggregate([
  {
  $group : {_id: {
    year :'$year',
    month : '$month_number'
  },
 
  total_volume : {$sum:'$total_flights'}
           }
  },
  
{$sort : { '_id.year' : 1,
         '_id.month':1} 
}

])


11.	Question 11: Write a diagnostic query to find all records where the number of departures is NOT EQUAL to the number of arrivals. (Use the Aggregations tab)

db.flights.aggregate([{


  $group :{_id:'$country',
           departs:{$sum:'$departures'},
           arrivals:{$sum:'$arrivals'}


}
},
  {$match: {expr:{$ne :['$departs','$arrivals']}}}
                    
                    
                     ])


