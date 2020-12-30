$ rails new wildilife_tracker -d postgresql -T
$ cd myapp
$ rails db:create
$ bundle add rspec-rails
$ rails generate rspec:install
$ rails server


$ rails generate resource Animal common_name:string, latin_name:string kingdom:string
$ rails generate resource Sighting



#The API Stories
#The Forest Service is considering a proposal to place in conservancy a forest of virgin Douglas fir just outside of Portland, Oregon. Before they give the go-ahead, they need to do an environmental impact study. They've asked you to build an app so that the rangers can use to report wildlife sightings.

#1) Story: As the consumer of the API I can create an animal and save it in the database. An animal has the following information: common name, latin name, kingdom (mammal, insect, etc.).


$ rails generate resource Animal common_name:string latin_name:string kingdom:string

#to see routes included in resources :animals
$ rails routes
$ rails db:migrate

#2) Story: As the consumer of the API I can list all the animals in a database.
#Hint: Make a few animals using Rails Console

$ rails c
Animal.create common_name:'Dog', latin_name:'Canis familiaris', kingdom:'Animalia'
Animal.create common_name:'Cat', latin_name:'Felis catus', kingdom:'Animalia'
Animal.create common_name:'Canary', latin_name:'Crithagra flaviventris', kingdom:'Animalia'


#3) Story: As the consumer of the API I can update an animal in the database.

update method working!

{
        "animal":{
             "common_name": "Bird",
             "latin_name": "Pappagallo frosty",
             "kingdom": "Animalia"
        }
}

#4) Story: As the consumer of the API I can destroy a animal in the database.

DELETE localhost:3000/animals/4  Send (done)


#5) Story: As the consumer of the API I can create a sighting of an animal with date (use the datetime datatype), latitude and longitude.
#Hint: An animal has_many sightings. (rails g resource Sighting animal_id:integer ...)

$ rails generate resource Sighting animal_id:integer date:datetime latitude:string longitude:string

has_many :sightings
belongs_to :animal

rails c

lea = Animal.first

Sighting.create animal_id:integer date:datetime latitude:string longitude:string

lea.sightings.create date:'2016-05-01 23:12:00', latitude:'Texas', longitude:'Equatorina'


#6) Story: As the consumer of the API I can update an animal sighting in the database.

PATCH localhost:3000/sightings/1

{
        "sighting":{
        "date": "2021-01-01T23:12:00.000Z",
        "latitude": "Fruskiona Meridionale",
        "longitude": "Equatorina"
        }

}



#7) Story: As the consumer of the API I can destroy an animal sighting in the database.

delete method working, ID2 not present

#8) Story: As the consumer of the API, when I view a specific animal, I can also see a list sightings of that animal.

def show
    animal = Animal.find(params[:id])
    render json: animal.sightings

end

#9) Story: As the consumer of the API, I can run a report to list all sightings during a given time period.
Hint: Your controller can look something like this:

class SightingsController < ApplicationController
  def index
    sightings = Sighting.where(start_date: params[:start_date]..params[:end_date])
    render json: sightings
  end
end

Remember to add the start_date and end_date to what is permitted in your strong parameters method.