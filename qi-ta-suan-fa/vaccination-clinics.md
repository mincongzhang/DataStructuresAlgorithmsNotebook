### Vaccination Clinics

The world health organization wants to establish a total of B vaccination clinics across N cities to immunization people against fatal diseases. Every city must have at least 1 clinic, and a clinic can only vaccinate people in the same city where they live. The goal is to minimize the number of vaccination kits needed in the largest clinic. For example suppose you have.

1.       2 cities and
2.       7 clinics to be opened
3.       200,000 is the population of first city
4.       500,000 is the population of second city
5.       Two clinics can open in the first city and
6.       Five in the second. This way
7.       100,000 people can be immunized in each of the two clinics in first city, and
8.       100.000 people can be immunized in the each clinic in the second city
9.       So the maximum number of people to be immunized in the largest clinic is 10,000

Input:
Two integers in the first line, N, the number of cities, and B, the total number of clinics to be opened
Each of the following N lines contains an integer ai, the population of city i

Output:
One integer representing the maximum number of people to be immunized in any single clinic

Constraints:
```

1<=N<=500,000

N<=B<=2,000,000

1<=ai<=5,000,000

```

Sample input:

```
2 7

200000

500000
```

Sample output:

```
100000
```


```
#include <iostream>
#include <queue>
#include <math.h> 

struct City {
	int population;
	int clinics;
	int immunized_people;
	void updateImmunizedPeople(){
		//use ceil because you cannot split people
		immunized_people = ceil(double(population)/double(clinics)); 
	}

	City(){}
	City(int p):population(p),clinics(1),immunized_people(p){}
};

struct CityCompare {
	bool operator()(const City &city1, const City &city2){
		return(city1.immunized_people < city2.immunized_people);
	}
};

int main(){

	int city_num(0), clinic_num(0);
	std::priority_queue<City, std::vector<City>, CityCompare> pq;

	std::cin >> city_num >> clinic_num;
	if(city_num>clinic_num){
		std::cout<<"Error! city_num > clinic_num"<<std::endl;
		return 0;
	}

	for(int i = 0; i<city_num; i++){
		int population(0);
		std::cin >> population;

		// init city
		City city(population);

		//push to p_queue
		pq.push(city);
		
		//reduce total clinic_num
		clinic_num--;
	}

	//assign the remaining clinics to cities
	//to minimize the max immunized_people
	for (int i=0; i<clinic_num; i++){

		//get the city with max immunized people 
		City city = pq.top();
		city.clinics++;

		city.updateImmunizedPeople();

		pq.pop();
		pq.push(city);
	}

	// print the max
	std::cout << pq.top().immunized_people<<std::endl;

	system("pause");
	return EXIT_SUCCESS;
}
```