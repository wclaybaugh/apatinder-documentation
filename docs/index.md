# APA Tinder for Dogs Web Application

## About the project

### 1. Introduction


### 2. Problem Statement
APA Tinder for Dogs is a web application that enables adopters

This project is part of Harvard's AC297r Capstone Project course, partnered with APA!.

### 3. Web Architecture

## Installation and setup (MacOS)
Please follow these steps below to install and set up all the necessary components to run our web application in a production environment.

### 1. MySQL Installation
1. **Download MySQL:** If MySQL is not already installed on your computer, download MySQL Community Server from this link: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/).

2. **Install MySQL.** Use "Legacy Password Encryption" option, for version compatibility.

3. **Initialize database and start server:** In System Preferences > MySQL, click on "Initialize Database." Set a password (this will be your password for the default root user), and use "Legacy Password Encryption." When it is complete, press "Start MySQL Server."

4. **Log into MySQL:** In terminal, type `mysql -u root -p`. Then type the password you set during database initialization (Step 3). Then you will be able to log into MySQL in the terminal.

5. (Optional) **Create another database user:** In MySQL type `GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';`. Replace `username` and `password` with the user information you want to create.

6. **Create database:** In MySQL, type `CREATE DATABASE apa_tinder;`. `apa_tinder` is the name of the database. Then type `USE apa_tinder;` to start using `apa_tinder` database.

7. **Import sql files into MySQL:** We used sql dump file from APA! to build and test our web application. To import the dump file into your local MySQL, in the terminal (not in MySQL) type `mysql -u root -p apa_tinder < /Users/directory/to/dumpfile/apadump.sql`. Use the directory path for your sql dump file. To easily get the directory path information, you can drag and drop your sql dump file into the terminal. Next, import `super_dog_table.sql` to your MySQL, using the same procedure.

### 2. Model Backend setup
1. Navigate to `src/model-server` folder. To download required packages, in terminal run `pip install -r requirements.txt`.
2. Create `.env` file: in terminal, type `touch .env`. In the `.env` file, populate the following variables with values.
```
DATABASE_USER=
DATABASE_PASSWORD=
SECRET_KEY= #this can be anything
```
3. In terminal, type `./run.sh`. If the backend runs successfully, you will see `Running on localhost:xxxx` from the resulting message. In your web browser, navigate to `localhost:xxxx` to view a list of model backend REST API endpoints.

### 3. Web Frontend and Backend setup
1. Install yarn: in terminal, type `brew install yarn`.
2. Next, you need to run the web backend server (GraphQL). Navigate to `src/apa-match-server/src`. In the directory, create `.env` file: in terminal, type `touch .env`. In the `.env` file, populate these variables with values. For example, for development and testing purposes, `DB_ENDPOINT=localhost`.
```
DB_ENDPOINT=
DB_USERNAME=
DB_PASSWORD=
TWILIO_SID=
TWILIO_PHONE=
TWILIO_TOKEN=
JWT_TOKEN=
MAILGUN_VALIDATION=
MAILGUN_API_KEY=
MAILGUN_DOMAIN=
```

3. Navigate to `src/apa-match-server`. In terminal, run `yarn install`, and then run `yarn run dev`. Confirm that you get the message "listening on port yyyy."
4. Now you will be running the web frontend. Navigate to `src/apa-match-client`. In terminal, run `yarn install`, and then run `yarn start`. Check the port number (zzzz) from the message. In the web browser, navigate to `localhost:zzzz` to view the web application.


## Dog Recommendation Model
### Data Cleaning
Our team was given sql data from APA! that contains information about all dogs (~30,000 dogs) in the shelter. There are 147 columns (147 different features for dogs) in the data, but the majority of columns was filled with NaN values or did not seem to be important for our model. We decided to drop columns where more than 60% of their values consist of NaNs and drop columns not relevant to our model, such as 'AnimalCoverPhoto', 'Location', 'AnimalStatus', etc. We were left with 21 columns, as shown below.

1. AnimalSex
2. AnimalCurrentWeightPounds
3. AnimalAltered
4. AnimalAgeDays
5. AnimalBreed
6. AnimalColor
7. AgeAtIntake
8. LastEventType
9. FirstIntakeEventType
10. LastIntakeEventType
11. CurrentVolunteerCategory
12. CurrentBehaviorCategory
13. IsDistemper
14. IsDistemperWatch
15. IsMedicalConsult
16. IsCGC
17. IsTopDog
18. IsHWPos
19. IsHWTreatment
20. IsBehaviorConsult
21. DaysInShelter

We cleaned each column to make it ready for modeling. While the majority of columns needed minor changes like dropping few NaN values, we spent significant amount of time inspecting 'AnimalColor' and 'AnimalBreed' columns, since they are important features when determining the popularity of dogs.

### Feature Engineering

#### AnimalColor column
There were 292 unique animal colors for around 30,000 dogs in our data, where animal colors such as Blue/Black, Blue/Gold, and Blue/Grey were classified as different colors. We decided to choose top 10 popular colors, Black, Brown, Tan, White, Cream, Blue, Blond, Red, Grey, and Yellow, and grouped all dogs into those 10 colors.

Instead of doing one-hot encoding for 10 different colors, we manually created 10 new columns indicating whether each dog contains each color - IsBlack, IsBrown, IsTan, IsWhite, IsCream, IsBlue, IsBlond, IsRed, IsGrey, and IsYellow. For example, if the color of a dog is Black/Brown, it will have values 1 for IsBlack and IsBrown columns, and 0 for the other columns. This approach is better than one-hot encoding, since one-hot encoding allows only one input to have a value 1. If we used one-hot encoding, this dog would have had a value 1 for only IsBlack or IsBrown column and 0 for the rest of columns, so it is not an accurate representation of our data.  

#### AnimalBreed column
There were 1969 unique animal breeds for around 30,000 dogs in our data, where animal breeds such as Beagle/Bulldog, Beagle/Dachshund, and Beagle/Hound were classified as different breeds. We decided to choose top 40 popular breeds obtained from APA! website as different breeds.

Instead of doing one-hot encoding for 40 different breeds, we manually created 40 new columns indicating whether each dog contains each breed - IsDachshund, IsBeagle, IsBulldog, etc. For example if the breed of a dog is Beagle/Bulldog, it will have values 1 for IsBeagle and IsBulldog columns, and 0 for the other columns. Again, if we used one-hot encoding, this dog would have had a value 1 for only IsBeagle or IsBulldog column and 0 for the rest of columns, so it is not an accurate representation of our data.  


### 1. Dog Popularity Model (Prior Model)

In APA!, it is important to predict popularity of each dog and how long each dog will stay in the shelter, since it wants to show right dogs to right people. While more popular dogs will have lower values for length of stay and less popular dogs will have higher values for lengthy of stay, the bayesian prior model uses dog features (age, color, breed, etc) to predict the length of stay for each dog.




### 2. User Swipes Model (Posterior Model)


## About Us
We are a group of Masters and PhD candidate students from Harvard University School of Engineering and Applied Sciences (SEAS). This project is part of AC297r: Capstone project class, and we have partnered with Austin Pets Alive! to work on this project.

## Acknowledgements
We thank our course instructors, Dr.David Sondak and Patrick Ohimoba, for guidance and support. We also thank Steve Porter from Austin Pets Alive! for making it possible for us to work on this project.