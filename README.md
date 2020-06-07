# Final Project Submission

## Please fill out:
* Student name:
    - Carl Cook
* Student pace: 
    - Part-Time
* Scheduled project review date/time: 
    - NONE
* Instructor name: 
    - James Irving
* Blog post URL:
    - NONE

# Questions

## What genres perform the best?

### TMDB API

Here we use the TMDB API to gather our own information to work with. This is probably our best information source.


```python
import json
import pandas as pd
import requests
import matplotlib.pyplot as plt
```

#### Function to supply API key


```python
def get_key(path):
    with open(path) as f:
        return json.load(f)
```


```python
key = get_key("/Users/katma/.secret/tmdb_api.json")
api_key = key["api_key"]
```

#### Building DataFrame


```python
columns = ['title', 'revenue', 'budget', 'genres', 'year', 'date']
df = pd.DataFrame(columns=columns)
years = ['2010', '2011', '2012', '2013', '2014', '2015', '2016', '2017', '2018', '2019']
```

#### Filling DataFrame


```python
# Loops from 2010 to 2019
for x in years:
    print(x)
    page = 1
    # Grabs first 10 pages from API
    while page <= 10:
        url = 'https://api.themoviedb.org/3/discover/movie?api_key=' + api_key + \
            '&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=' + \
            str(page) + '&primary_release_year=' + str(x)
        print(url)
        by_year = requests.get(url)
        year = by_year.json()
        pages = year['total_pages']
        results = year['results']
        page += 1
        # Pulls information for each film on the current page
        for film in results:
            print(film['title'])
            try:
                film_rev = requests.get('https://api.themoviedb.org/3/movie/' + str(film['id']) +\
                                        '?api_key=' + api_key + '&language=en-US')
                film_rev = film_rev.json()
                details = [film['title'], film_rev['revenue'], film_rev['budget'],\
                           [x['name'] for x in film_rev['genres']], film_rev['release_date'][:4],\
                           film_rev['release_date'][5:]]
                df.loc[len(df)]=details
            except:
                continue
```

    2010
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2010
    Toy Story 3
    Alice in Wonderland
    Harry Potter and the Deathly Hallows: Part 1
    Inception
    Shrek Forever After
    The Twilight Saga: Eclipse
    Iron Man 2
    Tangled
    Despicable Me
    How to Train Your Dragon
    The Chronicles of Narnia: The Voyage of the Dawn Treader
    The King's Speech
    TRON: Legacy
    The Karate Kid
    Tom and Jerry Meet Sherlock Holmes
    Prince of Persia: The Sands of Time
    Black Swan
    Megamind
    The Last Airbender
    Robin Hood
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2010
    Little Fockers
    Resident Evil: Afterlife
    Shutter Island
    Salt
    Sex and the City 2
    The Tourist
    The Expendables
    Grown Ups
    Knight and Day
    True Grit
    Gulliver's Travels
    Clash of the Titans
    Percy Jackson & the Olympians: The Lightning Thief
    The Social Network
    Valentine's Day
    The Sorcerer's Apprentice
    Due Date
    Eat Pray Love
    Yogi Bear
    Paranormal Activity 2
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2010
    The A-Team
    The Other Guys
    Unstoppable
    Step Up 3D
    The Book of Eli
    The Town
    Date Night
    The Secret World of Arrietty
    Legend of the Guardians: The Owls of Ga'Hoole
    Saw 3D
    The Bounty Hunter
    Wall Street: Money Never Sleeps
    Predators
    Nanny McPhee and the Big Bang
    Jackass 3D
    A Nightmare on Elm Street
    Dear John
    Cats & Dogs: The Revenge of Kitty Galore
    Tooth Fairy
    Life As We Know It
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2010
    Hereafter
    Love & Other Drugs
    The Other Woman
    Aftershock
    Killers
    Insidious
    Kick-Ass
    Green Zone
    The Fighter
    Get Him to the Greek
    Burlesque
    Dinner for Schmucks
    Marmaduke
    Doraemon: Nobita's Great Battle of the Mermaid King
    Piranha 3D
    Letters to Juliet
    Vampires Suck
    The Back-Up Plan
    Easy A
    Edge of Darkness
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2010
    Bayside Shakedown 3: Set the Guys Loose
    RED
    A Turtle's Tale: Sammy's Adventures
    Takers
    Legion
    The Last Exorcism
    Skyline
    Welcome to the South
    The American
    Hot Tub Time Machine
    Elite Squad: The Enemy Within
    Enthiran
    Secretariat
    The Ghost Writer
    Why Did I Get Married Too?
    Morning Glory
    Remember Me
    Cop Out
    The Crazies
    From Paris with Love
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2010
    Detective Dee and the Mystery of the Phantom Flame
    The Next Three Days
    Alpha and Omega
    The Switch
    She's Out of My League
    How Do You Know
    Little White Lies
    Charlie St. Cloud
    Scott Pilgrim vs. the World
    Heartbreaker
    The Debt
    The Spy Next Door
    Machete
    The Man from Nowhere
    When in Rome
    My Name Is Khan
    Going the Distance
    Of Gods and Men
    For Colored Girls
    Don't Be Afraid of the Dark
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2010
    Furry Vengeance
    Ip Man 2
    127 Hours
    The Kids Are All Right
    The Extraordinary Adventures of Adèle Blanc-Sec
    Devil
    You Again
    Leap Year
    Ramona and Beezus
    The Round Up
    Biutiful
    Fair Game
    Let Me In
    The Losers
    Faster
    Recep Ivedik 3
    Our Russia. Eggs of Destiny
    Just Wright
    Our Family Wedding
    71: Into the Fire
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2010
    Country Strong
    The Way Back
    Ayirathil Oruvan
    My Soul to Take
    Buried
    Three Heroes and the Shamakhan Queen
    Repo Men
    StreetDance 3D
    Another Year
    Norwegian Wood
    13 Assassins
    Sarah's Key
    The Last Godfather
    Blue Valentine
    Tomorrow, When the War Began
    Barbie in A Mermaid Tale
    The Nutcracker
    The Conspirator
    Kandahar
    Extraordinary Measures
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2010
    Winter's Bone
    The Way
    In a Better World
    Julia's Eyes
    Made in Dagenham
    Cho Kamen Rider Den-O Trilogy - Episode Red: ZeronoStar Twinkle
    What Men Talk About
    Cho Kamen Rider Den-O Trilogy - Episode Blue: The Dispatched Imagin is Newtral
    I Hate Luv Storys
    Vinnaithaandi Varuvaayaa
    Cho Kamen Rider Den-O Trilogy - Episode Yellow: Treasure de End Pirates
    Tamara Drewe
    Space Battleship Yamato
    The Warrior's Way
    Halo: Legends
    Jonah Hex
    Love and the City 2
    Thillalangadi
    Eesan
    Milaga
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2010
    Chikku Bhukku
    Khatta Meetha
    Never Let Me Go
    Ghost
    Singam
    Raavanan
    Stone
    MacGruber
    Boy
    Vedam
    Fatal
    We Are from the Future 2
    New Kids Turbo
    Barney's Version
    Outrage
    Once Upon a Time in Mumbaai
    Loose Cannons
    Senna
    Inside Job
    Last Night
    2011
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2011
    Harry Potter and the Deathly Hallows: Part 2
    Transformers: Dark of the Moon
    Pirates of the Caribbean: On Stranger Tides
    The Twilight Saga: Breaking Dawn - Part 1
    Mission: Impossible - Ghost Protocol
    Kung Fu Panda 2
    Fast Five
    The Smurfs
    Cars 2
    Puss in Boots
    Rio
    Rise of the Planet of the Apes
    Thor
    The Intouchables
    The Adventures of Tintin
    Captain America: The First Avenger
    X-Men: First Class
    Alvin and the Chipmunks: Chipwrecked
    Sherlock Holmes: A Game of Shadows
    Real Steel
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2011
    Bridesmaids
    Super 8
    The Hangover Part II
    Rango
    The Girl with the Dragon Tattoo
    The Green Hornet
    Immortals
    Green Lantern
    The Help
    Bad Teacher
    Just Go with It
    Horrible Bosses
    Paranormal Activity 3
    Battle: Los Angeles
    Gnomeo & Juliet
    Mr. Popper's Penguins
    Hugo
    Hop
    War Horse
    The Descendants
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2011
    Cowboys & Aliens
    In Time
    Zookeeper
    The Muppets
    Limitless
    Johnny English Reborn
    Zindagi Na Milegi Dobara
    Final Destination 5
    Tower Heist
    Midnight in Paris
    Friends with Benefits
    Happy Feet Two
    I Am Number Four
    Jack and Jill
    No Strings Attached
    Source Code
    Crazy, Stupid, Love.
    New Year's Eve
    Contagion
    The Best Exotic Marigold Hotel
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2011
    The Artist
    Ghost Rider: Spirit of Vengeance
    The Three Musketeers
    Unknown
    The Adjustment Bureau
    We Bought a Zoo
    Water for Elephants
    The Iron Lady
    Moneyball
    Sanctum
    7Aum Arivu
    Justin Bieber: Never Say Never
    Paul
    Scream 4
    The Rite
    Dolphin Tale
    The Flowers of War
    Season of the Witch
    Sucker Punch
    Red Riding Hood
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2011
    The Inbetweeners Movie
    Hall Pass
    Spy Kids: All the Time in the World
    The Lincoln Lawyer
    J. Edgar
    Big Mommas: Like Father, Like Son
    Abduction
    Tinker Tailor Soldier Spy
    Priest
    Drive
    The Grey
    The Ides of March
    The Change-Up
    The Dilemma
    Something Borrowed
    The Darkest Hour
    Hanna
    Footloose
    From Up on Poppy Hill
    Colombiana
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2011
    One Day
    Killer Elite
    Extremely Loud & Incredibly Close
    The Tree of Life
    Madea's Big Happy Family
    The Roommate
    The Mechanic
    War of the Arrows
    Conan the Barbarian
    Sunny
    Soul Surfer
    Arthur
    Beastly
    Kokowääh
    Fright Night
    30 Minutes or Less
    Monte Carlo
    50/50
    Oosaravelli
    Mars Needs Moms
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2011
    Dream House
    Jumping the Broom
    Larry Crowne
    A Very Harold & Kumar Christmas
    My Week with Marilyn
    The Sitter
    Jane Eyre
    Courageous
    Punch
    Shaolin
    Haywire
    Detective K: Secret of Virtuous Widow
    Silenced
    I Don't Know How She Does It
    What's Your Number?
    A Few Best Men
    Warriors of the Rainbow: Seediq Bale - Part 2: The Rainbow Bridge
    Drive Angry
    The Thing
    Mankatha
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2011
    Carnage
    A Dangerous Method
    The Eagle
    Ready
    Your Highness
    Apollo 18
    A Lonely Place to Die
    Singham
    Our Idiot Brother
    A Separation
    From Iran, a Separation
    The Rum Diary
    The Lost Bladesman
    Warrior
    Young Adult
    Polisse
    Kaavalan
    Rockstar
    The Guard
    Margin Call
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2011
    Shame
    Glee: The Concert Movie
    Mere Brother Ki Dulhan
    Delhi Belly
    Gooische Vrouwen
    What Men Still Talk About
    The Dirty Picture
    Melancholia
    Headhunters
    Anonymous
    African Cats
    Pina
    Winnie the Pooh
    You're Next
    Beginners
    Judy Moody and the Not Bummer Summer
    The Story Of Short Stack
    X-Large
    Shadow Boxing 3. The Final Round
    Hoodwinked Too! Hood VS. Evil
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2011
    Leafie, a Hen Into the Wild
    Silent House
    The Worst Week of My Life
    Seeking Justice
    This Must Be the Place
    We Need to Talk About Kevin
    Win Win
    Straw Dogs
    Prom
    Shark Night 3D
    The Best Movie 3-DE
    Delicacy
    Trespass
    Hysteria
    Guo Ming Yi
    Dum Maaro Dum
    Beremennyy
    Hollywood & Wine
    Aarakshan
    Born to Be Wild
    2012
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2012
    The Avengers
    Skyfall
    The Dark Knight Rises
    The Hobbit: An Unexpected Journey
    Ice Age: Continental Drift
    The Twilight Saga: Breaking Dawn - Part 2
    The Amazing Spider-Man
    Madagascar 3: Europe's Most Wanted
    The Hunger Games
    Men in Black 3
    Life of Pi
    Ted
    Brave
    Wreck-It Ralph
    Les Misérables
    Django Unchained
    Prometheus
    Snow White and the Huntsman
    Taken 2
    Hotel Transylvania
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2012
    Journey 2: The Mysterious Island
    The Lorax
    The Expendables 2
    Rise of the Guardians
    Battleship
    Wrath of the Titans
    John Carter
    The Bourne Legacy
    Lincoln
    Dark Shadows
    Resident Evil: Retribution
    Silver Linings Playbook
    American Reunion
    Argo
    Jack Reacher
    Safe House
    Lost in Thailand
    21 Jump Street
    Total Recall
    The Impossible
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2012
    The Vow
    Mirror Mirror
    The Dictator
    Magic Mike
    Flight
    Underworld: Awakening
    This Means War
    Chronicle
    Paranormal Activity 4
    Step Up Revolution
    Zero Dark Thirty
    Cloud Atlas
    The Woman in Black
    Parental Guidance
    The Pirates! In an Adventure with Scientists!
    Abraham Lincoln: Vampire Hunter
    Pitch Perfect
    Hope Springs
    ParaNorman
    The Campaign
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2012
    Painted Skin: The Resurrection
    The Devil Inside
    Someone Wants to Talk with You
    Project X
    The Lucky One
    Think Like a Man
    This Is 40
    The Possession
    The Thieves
    Savages
    Sinister
    Act of Valor
    What to Expect When You're Expecting
    To Rome with Love
    Here Comes the Boom
    The Cabin in the Woods
    Anna Karenina
    The Watch
    Moonrise Kingdom
    Madea's Witness Protection
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2012
    Contraband
    Rurouni Kenshin Part I: Origins
    Evangelion: 3.0 You Can (Not) Redo
    Quartet
    Rock of Ages
    That's My Boy
    The Odd Life of Timothy Green
    The Three Stooges
    The Five-Year Engagement
    Lawless
    Silent Hill: Revelation 3D
    Ek Tha Tiger
    Red Tails
    HOUBA! On the Trail of the Marsupilami
    End of Watch
    Looper
    Man on a Ledge
    Red Dawn
    House at the End of the Street
    Pokémon the Movie: Kyurem vs. the Sword of Justice
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2012
    The Guilt Trip
    Dredd
    Would I Lie to You? 3
    Safe
    Killing Them Softly
    One for the Money
    The Place Beyond the Pines
    Arbitrage
    Always - Sunset on Third Street 3
    Frankenweenie
    Welcome to the North
    Chimpanzee
    Salmon Fishing in the Yemen
    Cirque du Soleil: Worlds Away
    2016: Obama's America
    The Perks of Being a Wallflower
    Seven Psychopaths
    Katy Perry: Part of Me
    Bait
    Lockout
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2012
    Premium Rush
    Rowdy Rathore
    Alex Cross
    Amour
    The Raven
    My Way
    The Master
    Thuppakki
    Agneepath
    Rust and Bone
    On the Other Side of the Tracks
    Big Miracle
    Sparkle
    Wanderlust
    Hitchcock
    Tad, the Lost Explorer
    A Thousand Words
    Beasts of the Southern Wild
    The Company You Keep
    Eega
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2012
    The Hunt
    Chernobyl Diaries
    Gone
    Cocktail
    Naruto Shippuden the Movie: Road to Ninja
    Niko 2: Little Brother, Big Trouble
    The Cold Light of Day
    Love Is All You Need
    Outrage Beyond
    Gabbar Singh
    Kahaani
    The Man with the Iron Fists
    Barbie in A Mermaid Tale 2
    Hit & Run
    Raaz 3
    Student of the Year
    Red Lights
    The Words
    Soulless
    People Like Us
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2012
    The Snow Queen
    Friends with Kids
    Bachelorette
    In the House
    Mommies, Happy New Year!
    English Vinglish
    Fun Size
    Haute Cuisine
    [REC]³ Genesis
    The Worst Christmas of My Life
    Gladiators of Rome
    That still Karloson!
    Seeking a Friend for the End of the World
    The Apparition
    For Greater Glory: The True Story of Cristiada
    Bernie
    The Sessions
    Ruby Sparks
    Conquest 1453
    Bodyguard
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2012
    Tutto tutto niente niente
    An Hour and a Half
    Hyde Park on Hudson
    On the Road
    Piranha 3DD
    Bel Ami
    Promised Land
    Iron Sky
    Ernest & Celestine
    Upside Down
    The Famous Five
    A Royal Affair
    A Company Man
    All Together
    White Tiger
    Jism 2
    The Angels' Share
    Legatee
    Barbara
    The Collection
    2013
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2013
    Frozen
    Iron Man 3
    Despicable Me 2
    The Hobbit: The Desolation of Smaug
    The Hunger Games: Catching Fire
    Fast & Furious 6
    Monsters University
    Gravity
    Man of Steel
    Thor: The Dark World
    The Croods
    World War Z
    Oz the Great and Powerful
    Star Trek Into Darkness
    The Wolverine
    Pacific Rim
    The Wolf of Wall Street
    G.I. Joe: Retaliation
    The Hangover Part III
    Now You See Me
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2013
    The Great Gatsby
    The Smurfs 2
    The Conjuring
    A Good Day to Die Hard
    Oblivion
    Elysium
    Turbo
    We're the Millers
    Epic
    American Hustle
    Cloudy with a Chance of Meatballs 2
    Grown Ups 2
    After Earth
    Hansel & Gretel: Witch Hunters
    Planes
    White House Down
    Percy Jackson: Sea of Monsters
    Jack the Giant Slayer
    The Secret Life of Walter Mitty
    12 Years a Slave
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2013
    Identity Thief
    Anchorman 2: The Legend Continues
    Olympus Has Fallen
    The Heat
    Jackass Presents: Bad Grandpa
    47 Ronin
    Lone Survivor
    RED 2
    Mama
    Last Vegas
    2 Guns
    Walking with Dinosaurs
    This Is the End
    Ender's Game
    Escape Plan
    Prisoners
    The Wind Rises
    Warm Bodies
    So Young
    The Butler
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2013
    Saving Mr. Banks
    Free Birds
    Gangster Squad
    Philomena
    Beyond: Two Souls
    Blue Jasmine
    Instructions Not Included
    Riddick
    Safe Haven
    Evil Dead
    The Mortal Instruments: City of Bones
    42
    Captain Phillips
    Dhoom 3
    Rush
    The Purge
    The Lone Ranger
    American Dreams in China
    Pain & Gain
    About Time
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2013
    Snowpiercer
    The Eternal Zero
    Carrie
    Finding Mr. Right
    Police Story 2013
    Scary Movie 5
    The Book Thief
    Escape from Planet Earth
    August: Osage County
    Young Detective Dee: Rise of the Sea Dragon
    The Best Man Holiday
    The Counselor
    The Call
    Stalingrad
    The House of Magic
    The Grandmaster
    Begin Again
    Side Effects
    The Host
    Runner Runner
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2013
    Chennai Express
    R.I.P.D.
    Kick-Ass 2
    A Haunted House
    Beautiful Creatures
    The Physician
    Dallas Buyers Club
    A Madea Christmas
    Delivery Man
    Vishwaroopam
    Dragon Ball Z: Battle of Gods
    The Berlin File
    21 & Over
    Her
    Parker
    The World's End
    Yeh Jawaani Hai Deewani
    Grudge Match
    The Chef, The Actor, The Scoundrel
    Tarzan
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2013
    The Internship
    Homefront
    Snitch
    The Family
    Jobs
    Underdogs
    Inside Llewyn Davis
    Movie 43
    Pokémon the Movie: Genesect and the Legend Awakened
    Kevin Hart: Let Me Explain
    Goliyon Ki Raasleela Ram-Leela
    Spring Breakers
    Minuscule: Valley of the Lost Ants
    Legend No. 17
    I Give It a Year
    The Incredible Burt Wonderstone
    Mandela: Long Walk to Freedom
    Khumba
    Enough Said
    Dark Skies
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2013
    Bhaag Milkha Bhaag
    Trance
    The Tale of the Princess Kaguya
    The Great Beauty
    The Way Way Back
    Baggage Claim
    The Railway Man
    The Big Wedding
    Diana
    Mud
    Arrambam
    My Mom is a Character: The Film
    Labor Day
    Like Father, Like Son
    The Flu
    Broken City
    The Best Offer
    The Bling Ring
    Grand Masti
    Admission
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2013
    Ivan Tsarevich & the Grey Wolf 2
    Dead Man Down
    The Attacks Of 26-11
    Special 26
    Nebraska
    Fruitvale Station
    The Lunchbox
    Space Pirate Captain Harlock
    Queen
    Battle of the Year
    Belle
    Aashiqui 2
    Behind the Candelabra
    Out of the Furnace
    A Boy Called H
    Ida
    The Last Exorcism Part II
    Machete Kills
    A Chilling Cosplay
    Paranoia
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2013
    Mirchi
    Puella Magi Madoka Magica the Movie Part III: Rebellion
    Louis & Luca and the Snow Machine
    Stoker
    Drishyam
    Action 3D
    Soodhu Kavvum
    I'm So Excited!
    Before Midnight
    The Princess
    Vanakkam Chennai
    Shootout at Wadala
    One Chance
    Cycling with Molière
    ¡Asu Mare!
    The Past
    Getaway
    Only God Forgives
    Nymphomaniac: Vol. I
    Blue Is the Warmest Color
    2014
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2014
    Transformers: Age of Extinction
    The Hobbit: The Battle of the Five Armies
    Guardians of the Galaxy
    Maleficent
    The Hunger Games: Mockingjay - Part 1
    X-Men: Days of Future Past - The Rogue Cut
    X-Men: Days of Future Past
    Captain America: The Winter Soldier
    Dawn of the Planet of the Apes
    The Amazing Spider-Man 2
    Interstellar
    Big Hero 6
    How to Train Your Dragon 2
    American Sniper
    Godzilla
    Rio 2
    Teenage Mutant Ninja Turtles
    The Lego Movie
    Kingsman: The Secret Service
    Penguins of Madagascar
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2014
    Edge of Tomorrow
    Gone Girl
    Night at the Museum: Secret of the Tomb
    Noah
    The Maze Runner
    300: Rise of an Empire
    Taken 3
    The Fault in Our Stars
    Divergent
    Mr. Peabody & Sherman
    Neighbors
    Exodus: Gods and Kings
    Paddington
    Annabelle
    Hercules
    RoboCop
    The Imitation Game
    Non-Stop
    Dracula Untold
    Into the Woods
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2014
    Fury
    The Expendables 3
    Need for Speed
    The Other Woman
    The Equalizer
    22 Jump Street
    Serial (Bad) Weddings
    The Grand Budapest Hotel
    Dumb and Dumber To
    Unbroken
    Into the Storm
    The Monuments Men
    Ride Along
    Planes: Fire & Rescue
    The Taking of Tiger Mountain
    Let's Be Cops
    Jack Ryan: Shadow Recruit
    Annie
    Lucy
    Sex Tape
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2014
    The Theory of Everything
    Blended
    The Nut Job
    PK
    Pompeii
    Seventh Son
    The Admiral: Roaring Currents
    The Purge: Anarchy
    Where Are We Going, Dad?
    The Boxtrolls
    Horrible Bosses 2
    The Breakup Guru
    Ode to My Father
    Ouija
    Birdman or (The Unexpected Virtue of Ignorance)
    Transcendence
    Heaven is for Real
    Alexander and the Terrible, Horrible, No Good, Very Bad Day
    Tammy
    George Strait: The Cowboy Rides Away
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2014
    The Book of Life
    The Hundred-Foot Journey
    John Wick
    Deliver Us from Evil
    A Million Ways to Die in the West
    Paranormal Activity: The Marked Ones
    Step Up All In
    The Judge
    Doraemon: Stand by Me Doraemon
    Muppets Most Wanted
    If I Stay
    Spanish Affair
    My Old Classmate
    I, Frankenstein
    Think Like a Man Too
    Brick Mansions
    Son of God
    Jersey Boys
    The Giver
    Selma
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2014
    The Legend of Hercules
    Kick
    St. Vincent
    No Good Deed
    A Walk Among the Tombstones
    3 Days to Kill
    Wild
    Dolphin Tale 2
    Magic in the Moonlight
    Nightcrawler
    Beauty and the Beast
    About Last Night
    Chef
    Earth to Echo
    Boyhood
    Oculus
    Still Alice
    The Salvation
    This Is Where I Leave You
    As Above, So Below
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2014
    Recep Ivedik 4
    Sin City: A Dame to Kill For
    The Gambler
    Million Dollar Arm
    Devil's Due
    Boonie Bears: To the Rescue
    The Best of Me
    When Marnie Was There
    Forbidden Empire
    Endless Love
    Get on Up
    The November Man
    The Four 3
    A Most Wanted Man
    Tazza: The Hidden Card
    Winter's Tale
    When the Game Stands Tall
    God's Not Dead
    Big Eyes
    Draft Day
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2014
    The Divine Move
    Wild Tales
    Grace of Monaco
    The Woman in Black 2: Angel of Death
    That Awkward Moment
    Iceman
    Top Five
    A Haunted House 2
    And So It Goes
    Jessabelle
    Mrs. Brown's Boys D'Movie
    2 States
    Black or White
    Yves Saint Laurent
    Tinker Bell and the Pirate Fairy
    Babysitting
    Left Behind
    The Drop
    Veeram
    Legends of Oz: Dorothy's Return
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2014
    Jilla
    The Quiet Ones
    Sabotage
    The Single Moms Club
    Get Married If You Can
    Mary Kom
    The Water Diviner
    Before I Go to Sleep
    Vampire Academy
    Parts Per Billion
    Inherent Vice
    Beyond the Lights
    Barbecue
    The Pyramid
    Island of Lemurs: Madagascar
    Whiplash
    Six Degrees of Celebration 1914
    The Interview
    Foxcatcher
    French Women
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2014
    A Most Violent Year
    The Man Cave
    Torrente 5
    Haider
    Moms' Night Out
    The Babadook
    The Loft
    Bo Burnham: Repeat Stuff
    Heropanti
    For Love or Money
    The Continent
    Suite Française
    The Magical Brush
    Velaiyilla Pattathari
    While We're Young
    My Old Lady
    Pride
    Big Game
    Jamais le premier soir
    A Long Way Down
    2015
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2015
    Star Wars: The Force Awakens
    Jurassic World
    Furious 7
    Avengers: Age of Ultron
    Minions
    Spectre
    Inside Out
    Mission: Impossible - Rogue Nation
    The Hunger Games: Mockingjay - Part 2
    The Martian
    Fifty Shades of Grey
    Cinderella
    The Revenant
    Ant-Man
    Hotel Transylvania 2
    San Andreas
    Terminator Genisys
    Mad Max: Fury Road (Black & Chrome)
    Mad Max: Fury Road
    Home
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2015
    The Good Dinosaur
    Maze Runner: The Scorch Trials
    The SpongeBob Movie: Sponge Out of Water
    Insurgent
    Pitch Perfect 2
    Mojin: The Lost Legend
    The Peanuts Movie
    Pixels
    Daddy's Home
    Spy
    Alvin and the Chipmunks: The Road Chip
    Ted 2
    Tomorrowland
    Everest
    Straight Outta Compton
    The Intern
    Jupiter Ascending
    Creed
    Fantastic Four
    Bridge of Spies
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2015
    Goosebumps
    Ip Man 3
    The Hateful Eight
    Focus
    Monkey King: Hero Is Back
    Bajrangi Bhaijaan
    The Last Witch Hunter
    Trainwreck
    Point Break
    The Big Short
    Pan
    Wolf Totem
    Dragon Blade
    Magic Mike XXL
    Get Hard
    The Man from U.N.C.L.E.
    Paul Blart: Mall Cop 2
    Shaun the Sheep Movie
    Sisters
    Chappie
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2015
    Vacation
    Insidious: Chapter 3
    Joy
    Chronicles of the Ghostly Tribe
    Black Mass
    The Visit
    The Little Prince
    Poltergeist
    In the Heart of the Sea
    Southpaw
    Bahubali: The Beginning
    Spotlight
    Veteran
    The Second Best Exotic Marigold Hotel
    Paper Towns
    Sicario
    Suck Me Shakespeer 2
    Hitman: Agent 47
    Wolf Warrior
    The Wedding Ringer
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2015
    Paranormal Activity: The Ghost Dimension
    Jawani Phir Nahi Ani
    Crimson Peak
    The Transporter Refueled
    Run All Night
    War Room
    Prem Ratan Dhan Payo
    The Age of Adaline
    The Danish Girl
    The Lazarus Effect
    The Longest Ride
    Unfriended
    Brooklyn
    Inside Men
    Krampus
    Dragon Ball Z: Resurrection 'F'
    Woman in Gold
    The Walk
    Dilwale
    The Perfect Guy
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2015
    The Gift
    Bajirao Mastani
    No Escape
    Sinister 2
    The Boy Next Door
    The Night Before
    Hot Pursuit
    Concussion
    Entourage
    Boonie Bears: Mystical Winter
    McFarland, USA
    Max
    The DUFF
    Detective Conan: Sunflowers of Inferno
    Legend
    The Gallows
    Love the Coopers
    The Lady in the Van
    Ricki and the Flash
    Gabbar Is Back
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2015
    The Witch
    Carol
    Spanish Affair 2
    Ex Machina
    Burnt
    A Walk in the Woods
    Room
    Secret in Their Eyes
    Steve Jobs
    Victor Frankenstein
    Doraemon: Nobita and the Space Heroes
    Project Almanac
    Self/less
    Mortdecai
    Far from the Madding Crowd
    Mr. Holmes
    Love & Mercy
    Irrational Man
    Detective K: Secret of the Lost Island
    American Ultra
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2015
    Aloha
    Look Who's Back
    The 33
    Babysitting 2
    Solace
    Piku
    Dil Dhadakne Do
    Love Live! The School Idol Movie
    The Clan
    Eye in the Sky
    The Dressmaker
    Dope
    Blackhat
    The Accidental Detective
    Regression
    Monkey Kingdom
    Macbeth
    Suffragette
    Yennai Arindhaal
    Mune: Guardian of the Moon
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2015
    The Lobster
    Scouts Guide to the Zombie Apocalypse
    It Follows
    Hello, My Name Is Doris
    Unfinished Business
    Woodlawn
    Hardcore Henry
    Papanasam
    The Gunman
    Strange Magic
    The Wave
    Hot Tub Time Machine 2
    Temper
    Badlapur
    We Are Your Friends
    The Tiger: An Old Hunter's Tale
    Mr Black: Green Star
    Danny Collins
    Kwai Boo
    A Second Chance
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2015
    Soulless 2
    Frau Müller muss weg!
    Huevos: Little Rooster's Egg-Cellent Adventure
    Beasts of No Nation
    Truman
    Hate Story 3
    Amy
    Capture the Flag
    Bhale Bhale Magadivoy
    Trumbo
    2 Countries
    Anegan
    Happy Little Submarine: Magic Box of Time
    Invisibles
    Ennu Ninte Moideen
    Tevar
    Dheepan
    The Battalion
    A Bigger Splash
    I'll See You in My Dreams
    2016
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2016
    Captain America: Civil War
    Rogue One: A Star Wars Story
    Finding Dory
    Zootopia
    The Jungle Book
    The Secret Life of Pets
    Batman v Superman: Dawn of Justice
    Fantastic Beasts and Where to Find Them
    Deadpool
    Suicide Squad
    Moana
    Doctor Strange
    Sing
    The Mermaid
    X-Men: Apocalypse
    Kung Fu Panda 3
    La La Land
    Warcraft
    Jason Bourne
    Ice Age: Collision Course
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2016
    Independence Day: Resurgence
    Your Name.
    The Legend of Tarzan
    The Angry Birds Movie
    Trolls
    Star Trek Beyond
    Now You See Me 2
    The Great Wall
    The Conjuring 2
    Resident Evil: The Final Chapter
    Dangal
    Passengers
    Alice Through the Looking Glass
    Miss Peregrine's Home for Peculiar Children
    Teenage Mutant Ninja Turtles: Out of the Shadows
    Sully
    Assassin's Creed
    Hidden Figures
    Ghostbusters
    Inferno
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2016
    Central Intelligence
    Bridget Jones's Baby
    Me Before You
    London Has Fallen
    Arrival
    The BFG
    The Monkey King 2
    A Aa
    Royal Treasure
    Bad Moms
    Storks
    Allegiant
    Hacksaw Ridge
    The Girl on the Train
    Operation Mekong
    The Huntsman: Winter's War
    The Magnificent Seven
    Jack Reacher: Never Go Back
    Don't Breathe
    The Accountant
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2016
    Gods of Egypt
    Lights Out
    Pete's Dragon
    Sausage Party
    Lion
    Mechanic: Resurrection
    Ride Along 2
    Deepwater Horizon
    Allied
    The Shallows
    The Purge: Election Year
    Why Him?
    Office Christmas Party
    How to Be Single
    The 5th Wave
    Neighbors 2: Sorority Rising
    10 Cloverfield Lane
    Cold War II
    Railroad Tigers
    Sultan
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2016
    Dirty Grandpa
    Ben-Hur
    Money Monster
    My Big Fat Greek Wedding 2
    Line Walker
    Collateral Beauty
    Train to Busan
    War Dogs
    Nerve
    Big Fish & Begonia
    Ouija: Origin of Evil
    Underworld: Blood Wars
    Manchester by the Sea
    The Boss
    Shin Godzilla
    Mike and Dave Need Wedding Dates
    Where Am I Going?
    Boo! A Madea Halloween
    Kabali
    Miracles from Heaven
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2016
    Kubo and the Two Strings
    13 Hours: The Secret Soldiers of Benghazi
    One Piece Film: GOLD
    Moonlight
    Monster Trucks
    Fences
    The Boy
    Hail, Caesar!
    The Nice Guys
    Skiptrace
    Nine Lives
    Detective Conan: The Darkest Nightmare
    Zoolander 2
    Barbershop: The Next Cut
    The Age of Shadows
    The Finest Hours
    Patriots Day
    Operation Chromite
    Florence Foster Jenkins
    Mother's Day
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2016
    A Monster Calls
    The Wailing
    Eddie the Eagle
    Risen
    Blair Witch
    Café Society
    League of Gods
    Almost Christmas
    Boonie Bears: The Big Top Secret
    Absolutely Fabulous: The Movie
    My Mom is a Character 2
    The Handmaiden
    The Forest
    Hell or High Water
    Snowden
    The Ten Commandments: The Movie
    R.A.I.D. Special Unit
    Airlift
    Billy Lynn's Long Halftime Walk
    When the Bough Breaks
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2016
    Pandora
    A Silent Voice
    The Invisible Guest
    Keeping Up with the Joneses
    Nocturnal Animals
    Masterminds
    Pulimurugan
    Fan
    Flight Crew
    Theri
    The Light Between Oceans
    Grimsby
    Free State of Jones
    Race
    A Beautiful Planet
    The Founder
    Silence
    Kevin Hart: What Now?
    God's Not Dead 2
    Middle School: The Worst Years of My Life
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2016
    Bad Santa 2
    Hunt for the Wilderpeople
    Whiskey Tango Foxtrot
    Live by Night
    Julieta
    Fifty Shades of Black
    Janatha Garage
    Keanu
    In This Corner of the World
    Sarrainodu
    The Edge of Seventeen
    The Choice
    Neerja
    Norm of the North
    The Perfect Weapon
    Pride and Prejudice and Zombies
    Mohenjo Daro
    The Birth of a Nation
    I, Daniel Blake
    Ki & Ka
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2016
    The Infiltrator
    Pink
    24
    Udta Punjab
    Criminal
    Bastille Day
    The Other Side of the Door
    Love & Friendship
    Jackie
    A United Kingdom
    Sing Street
    Shut In
    Hillary's America: The Secret History of the Democratic Party
    Triple 9
    No Manches Frida
    The Beatles: Eight Days a Week - The Touring Years
    Um Suburbano Sortudo
    The Man Who Knew Infinity
    The Darkness
    Queen of Katwe
    2017
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2017
    Star Wars: The Last Jedi
    Beauty and the Beast
    The Fate of the Furious
    Despicable Me 3
    Jumanji: Welcome to the Jungle
    Spider-Man: Homecoming
    Wolf Warrior 2
    Guardians of the Galaxy Vol. 2
    Thor: Ragnarok
    Wonder Woman
    Coco
    Pirates of the Caribbean: Dead Men Tell No Tales
    It
    Justice League
    Logan
    Transformers: The Last Knight
    Kong: Skull Island
    Dunkirk
    The Boss Baby
    War for the Planet of the Apes
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2017
    Chasing the Dragon
    The Greatest Showman
    Kingsman: The Golden Circle
    The Mummy
    Cars 3
    Fifty Shades Darker
    Murder on the Orient Express
    xXx: Return of Xander Cage
    The Lego Batman Movie
    Annabelle: Creation
    Wonder
    Ferdinand
    Split
    Split
    Baahubali 2: The Conclusion
    Blade Runner 2049
    Get Out
    Kung Fu Yoga
    Alien: Covenant
    Paddington 2
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2017
    Baby Driver
    Valerian and the City of a Thousand Planets
    Youth
    Geostorm
    The Emoji Movie
    One Cut of the Dead
    Smurfs: The Lost Village
    The Shape of Water
    A Dog's Purpose
    Pitch Perfect 3
    Daddy's Home 2
    Baywatch
    The Hitman's Bodyguard
    The Post
    John Wick: Chapter 2
    Ghost in the Shell
    Italy Italy
    Three Billboards Outside Ebbing, Missouri
    King Arthur: Legend of the Sword
    Darkest Hour
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2017
    The Foreigner
    Power Rangers
    Girls Trip
    Secret Superstar
    American Made
    A Bad Moms Christmas
    Along with the Gods: The Two Worlds
    Captain Underpants: The First Epic Movie
    The Lego Ninjago Movie
    Happy Death Day
    The Dark Tower
    Jigsaw
    Life
    Atomic Blonde
    The Shack
    Heartthrob
    Tiger Zinda Hai
    Going in Style
    Rings
    A Taxi Driver
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2017
    City of Rock
    Boonie Bears: Entangled Worlds
    Lady Bird
    American Assassin
    Victoria & Abdul
    The Nut Job 2: Nutty by Nature
    The Mountain Between Us
    Everything, Everything
    Snatched
    My Little Pony: The Movie
    Molly's Game
    All the Money in the World
    The Big Sick
    Confidential Assignment
    Downsizing
    I, Tonya
    The Outlaws
    Hindi Medium
    Three Seconds
    1987: When the Day Comes
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2017
    Logan Lucky
    Boo 2! A Madea Halloween
    Phantom Thread
    The Son of Bigfoot
    Flatliners
    Rough Night
    Wind River
    All Eyez on Me
    mother!
    47 Meters Down
    The Snowman
    Hanson and the Beast
    Loving Vincent
    T2 Trainspotting
    Fist Fight
    Call Me by Your Name
    Gifted
    Diary of a Wimpy Kid: The Long Haul
    Judwaa 2
    Tad the Lost Explorer and the Secret of King Midas
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2017
    Steel Rain
    Sleepless
    Kidnap
    The Last Warrior
    The Disaster Artist
    Hostiles
    Alibi.com
    Recep Ivedik 5
    What Happened to Monday
    The Beguiled
    Home Again
    Baadshaho
    CHiPS
    The Bye Bye Man
    Fireworks, Should We See It from the Side or the Bottom?
    A Cure for Wellness
    Happy Family
    How to Be a Latin Lover
    Only the Brave
    Father Figures
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2017
    Ingrid Goes West
    The Death of Stalin
    C'est la vie!
    Detroit
    The Zookeeper's Wife
    Sword Art Online: The Movie - Ordinal Scale
    The Circle
    Attraction
    It Comes at Night
    Father and Son
    The Big Bad Fox and Other Tales
    Unforgettable
    The Humanity Bureau
    Loving Pablo
    Bairavaa
    Borg vs McEnroe
    Six Degrees of Celebration 6
    Seer Movie 6: Invincible Puni
    Unknown Soldier
    Finding Your Feet
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2017
    Wonder Wheel
    The Space Between Us
    Tall Tales from the Magical Garden of Antoon Krings
    Wish Upon
    Vasco Modena Park - Il film
    Toilet: A Love Story
    Salyut-7
    Megan Leavey
    Da Hu Fa
    Half Girlfriend
    Battle of the Sexes
    Jagga Jasoos
    Before I Fall
    Gautamiputra Satakarni
    Roman J. Israel, Esq.
    The Florida Project
    Wished
    Marrowbone
    A Bag of Marbles
    You Were Never Really Here
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2017
    Furious
    The Blacksmith
    Kamen Rider Heisei Generations FINAL: Build & Ex-Aid with Legend Riders
    Marshall
    Forgotten
    The Spacewalker
    The Glass Castle
    The Great Father
    Thank You for Your Service
    Kamen Rider Ex-Aid the Movie: True Ending
    My Cousin Rachel
    Çalgı Çengi İkimiz
    Nenu Local
    Shubh Mangal Saavdhan
    Fate/Stay Night: Heaven's Feel I. Presage Flower
    Stronger
    Condorito: The Movie
    Shatamanam Bhavati
    Bareilly Ki Barfi
    Arjun Reddy
    2018
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2018
    Red Dead Redemption II
    Avengers: Infinity War
    Black Panther
    Jurassic World: Fallen Kingdom
    Incredibles 2
    Aquaman
    In The Mood For Surrealism or 8021
    Bohemian Rhapsody
    Venom
    Mission: Impossible - Fallout
    Deadpool 2
    Fantastic Beasts: The Crimes of Grindelwald
    Ant-Man and the Wasp
    Ready Player One
    Operation Red Sea
    The Meg
    Ralph Breaks the Internet
    The Grinch
    Bumblebee
    A Star Is Born
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2018
    Rampage
    Mamma Mia! Here We Go Again
    Solo: A Star Wars Story
    Spider-Man: Into the Spider-Verse
    Fifty Shades Freed
    The Nun
    Peter Rabbit
    Mary Poppins Returns
    A Quiet Place
    Green Book
    Ocean's Eight
    Skyscraper
    Pacific Rim: Uprising
    Maze Runner: The Death Cure
    Tomb Raider
    Halloween
    Crazy Rich Asians
    Creed II
    Us and Them
    Bel Canto
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2018
    The Mule
    Insidious: The Last Key
    The Predator
    Johnny English Strikes Again
    Red Sparrow
    The First Purge
    A Wrinkle in Time
    The House with a Clock in Its Walls
    Dragon Ball Super: Broly
    The Commuter
    Game Night
    2.0
    Along with the Gods: The Last 49 Days
    The Equalizer 2
    First Man
    Hotel Transylvania 3: Summer Vacation
    Taxiwala
    Khaltoor
    Christopher Robin
    A Simple Favor
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2018
    Truth or Dare
    Blockers
    Goosebumps 2: Haunted Halloween
    Kamen Rider Build the Movie: Be The One
    Overboard
    Boonie Bears: The Big Shrink
    Sherlock Gnomes
    I Can Only Imagine
    Detective Dee: The Four Heavenly Kings
    Mortal Engines
    The Favourite
    Padmaavat
    Den of Thieves
    Hereditary
    Book Club
    Vice
    Sicario: Day of the Soldado
    Tag
    Widows
    Robin Hood
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2018
    Second Act
    Sanju
    12 Strong
    Love, Simon
    Searching
    Capernaum
    Isle of Dogs
    Life of the Party
    The Nutcracker and the Four Realms
    The 15:17 to Paris
    Early Man
    I Feel Pretty
    Breaking In
    Peppermint
    BlacKkKlansman
    Asterix: The Secret of the Magic Potion
    Annihilation
    Overlord
    The Great Battle
    The Darkest Minds
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2018
    The Possession of Hannah Grace
    Aiyaary
    Holmes & Watson
    Andhadhun
    Badhaai Ho
    Show Dogs
    On the Basis of Sex
    Mary Queen of Scots
    Tanks for Stalin
    T-34
    Alpha
    Acrimony
    Death Wish
    The Spy Who Dumped Me
    Night School
    Bad Times at the El Royale
    My Hero Academia: Two Heroes
    The Strangers: Prey at Night
    Dark Figure of Crime
    The Hate U Give
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2018
    Nobody's Fool
    Teen Titans Go! To the Movies
    Mirai
    The Happytime Murders
    Midnight Sun
    White Boy Rick
    Hichki
    Alad'2
    Winchester
    The Witch: Part 1. The Subversion
    The Accidental Detective 2: In Action
    Won't You Be My Neighbor?
    Paul, Apostle of Christ
    Master Z: Ip Man Legacy
    Free Solo
    Proud Mary
    Unbound
    They Shall Not Grow Old
    On Your Wedding Day
    If Beale Street Could Talk
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2018
    SuperFly
    Champions
    Veere Di Wedding
    Oolong Courtyard: Kung Fu School
    Super Troopers 2
    Cold War
    Detective K: Secret of the Living Dead
    Be with You
    Everybody Knows
    Chappaquiddick
    The Girl in the Spider's Web
    Uncle Drew
    Operation Finale
    Upgrade
    Forever My Girl
    Tully
    Crayon Shin-chan: Burst Serving! Kung Fu Boys ~Ramen Rebellion~
    Muslum
    Instant Family
    Unsane
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2018
    Colette
    Eighth Grade
    Welcome to Marwen
    Superlopez
    Hotel Artemis
    Crystal Sky of Yesterday
    Racetime
    Slender Man
    Mahanati
    The Old Man & the Gun
    Mary Magdalene
    Mirage
    Death House
    El Angel
    The Miracle Season
    Kin
    Unfriended: Dark Web
    Batti Gul Meter Chalu
    7 Days in Entebbe
    Sauerkraut Coma
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2018
    Can You Ever Forgive Me?
    The Vegetable
    There Is No Place Like Home
    Replicas
    Life Itself
    Boy Erased
    Beautiful Boy
    Nice to Meet You
    Gogol. Viy
    Beirut
    mid90s
    Burning
    Suspiria
    Champion
    Every Day
    Pari
    God's Not Dead: A Light in Darkness
    The Wife
    The House That Jack Built
    Hello Guru Prema Kosame
    2019
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=1&primary_release_year=2019
    Avengers: Endgame
    The Lion King
    Frozen II
    Spider-Man: Far from Home
    Captain Marvel
    Joker
    Star Wars: The Rise of Skywalker
    Toy Story 4
    Aladdin
    Fast & Furious Presents: Hobbs & Shaw
    Ne Zha
    How to Train Your Dragon: The Hidden World
    Maleficent: Mistress of Evil
    It Chapter Two
    Pokémon Detective Pikachu
    Alita: Battle Angel
    Godzilla: King of the Monsters
    Once Upon a Time… in Hollywood
    1917
    Shazam!
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=2&primary_release_year=2019
    John Wick: Chapter 3 - Parabellum
    Jumanji: The Next Level
    Knives Out
    Terminator: Dark Fate
    Us
    Men in Black: International
    Dark Phoenix
    Glass
    Parasite
    Annabelle Comes Home
    Ford v Ferrari
    Dumbo
    The Addams Family
    Downton Abbey
    Little Women
    The Lego Movie 2: The Second Part
    Weathering with You
    Gemini Man
    Rocketman
    Hustlers
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=3&primary_release_year=2019
    Escape Room
    The Secret Life of Pets 2
    Angel Has Fallen
    Ad Astra
    The Angry Birds Movie 2
    Midway
    Zombieland: Double Tap
    The Curse of La Llorona
    Last Christmas
    Extreme Job
    Wonder Park
    Dora and the Lost City of Gold
    Pet Sematary
    Abominable
    The Upside
    Scary Stories to Tell in the Dark
    Rambo: Last Blood
    Crawl
    Yesterday
    Jojo Rabbit
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=4&primary_release_year=2019
    Serial (Bad) Weddings 2
    Cats
    Charlie's Angels
    Good Boys
    Doctor Sleep
    What Men Want
    After
    EXIT
    A Beautiful Day in the Neighborhood
    White Snake
    War
    Cold Pursuit
    Ready or Not
    Kabir Singh
    Saaho
    Long Shot
    Bombshell
    Ma
    Just Mercy
    Uncut Gems
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=5&primary_release_year=2019
    21 Bridges
    Midsommar
    Queen & Slim
    A Madea Family Funeral
    Child's Play
    Hellboy
    Playing with Fire
    A Shaun the Sheep Movie: Farmageddon
    My Mom is a Character 3
    Fighting with My Family
    Judy
    Maharshi
    Gully Boy
    Richard Jewell
    The Art of Racing in the Rain
    The Good Liar
    Brightburn
    The Hustle
    The Kid Who Would Be King
    Anna
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=6&primary_release_year=2019
    My Hero Academia: Heroes Rising
    Pain and Glory
    Lucifer
    UglyDolls
    The Intruder
    Happy Death Day 2U
    The Gangster, the Cop, the Devil
    Booksmart
    Stuber
    Late Night
    The Farewell
    Shaft
    The Prodigy
    Motherless Brooklyn
    The Lighthouse
    A Dog's Way Home
    Little
    Hotel Mumbai
    Hello, Love, Goodbye
    Missing Link
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=7&primary_release_year=2019
    Tazza: One Eyed Jack
    Miss Bala
    Mao Zedong 1949
    Il primo Natale
    Recep Ivedik 6
    Miracle in Cell No. 7
    Serenity
    Dark Waters
    The Cabin House
    While at War
    The Dead Don't Die
    Sumikko Gurashi
    The Invincible Dragon
    The Divine Fury
    Venky Mama
    Money Trap
    Just Retired
    Portrait of a Lady on Fire
    Women on the Run
    The Panti Sisters
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=8&primary_release_year=2019
    Tolkien
    Poms
    The Fortune Goddess
    The Irishman
    Battle of Jangsari
    10 Days without Mamma
    Brittany Runs a Marathon
    Le meilleur reste à venir
    Furie
    Billion
    Run the Race
    Captive State
    Blue Story
    Red Shoes and the Seven Dwarfs
    The Biggest Little Farm
    Natasaarvabhowma
    Don't Stop Me Now
    Balkan Line
    The Truth
    The Space Between The Lines
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=9&primary_release_year=2019
    All You Need is Crime
    A Hidden Life
    The Beach Bum
    The Shiny Shrimps
    Ittymaani: Made In China
    Odessa
    Spy Cat
    Driving Licence
    La Lutte des classes
    Keep Safe Distance
    The Last Black Man in San Francisco
    The Blackout: Invasion Earth
    You've Got Murder
    The Lodge
    Happy Birthday
    Vic the Viking and the Magic Sword
    A Dog's Journey
    Spirits in the Forest
    The Art of Self-Defense
    Eye for an Eye
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&page=10&primary_release_year=2019
    Coma
    The Silence
    Marriage Story
    Luce
    Honey Boy
    Adiós
    Danger Close
    Abigail
    The 9th Precinct
    Waves
    Martin Eden
    Kral Şakir: Korsanlar Diyarı
    Modalità aereo
    5Gang: A Different Kind of Christmas
    Stanley H.
    One Love , Two Lives
    Wrong No. 2
    El Chicano
    Chhalawa
    Roger Waters: Us + Them
    

#### Check the DataFrame


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>1066969703</td>
      <td>200000000</td>
      <td>[Animation, Family, Comedy]</td>
      <td>2010</td>
      <td>06-16</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland</td>
      <td>1025467110</td>
      <td>200000000</td>
      <td>[Family, Fantasy, Adventure]</td>
      <td>2010</td>
      <td>03-03</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>954305868</td>
      <td>250000000</td>
      <td>[Adventure, Fantasy]</td>
      <td>2010</td>
      <td>10-17</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>825532764</td>
      <td>160000000</td>
      <td>[Action, Science Fiction, Adventure]</td>
      <td>2010</td>
      <td>07-15</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>752600867</td>
      <td>165000000</td>
      <td>[Comedy, Adventure, Fantasy, Animation, Family]</td>
      <td>2010</td>
      <td>05-16</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>1991</td>
      <td>One Love , Two Lives</td>
      <td>1401422</td>
      <td>0</td>
      <td>[Drama, Romance]</td>
      <td>2019</td>
      <td>02-15</td>
    </tr>
    <tr>
      <td>1992</td>
      <td>Wrong No. 2</td>
      <td>1400000</td>
      <td>0</td>
      <td>[Romance, Comedy]</td>
      <td>2019</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>1993</td>
      <td>El Chicano</td>
      <td>1370000</td>
      <td>0</td>
      <td>[Drama, Action, Crime]</td>
      <td>2019</td>
      <td>05-03</td>
    </tr>
    <tr>
      <td>1994</td>
      <td>Chhalawa</td>
      <td>1300000</td>
      <td>0</td>
      <td>[Comedy, Drama, Family, Romance]</td>
      <td>2019</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>1995</td>
      <td>Roger Waters: Us + Them</td>
      <td>1294480</td>
      <td>0</td>
      <td>[Documentary, Music]</td>
      <td>2019</td>
      <td>10-02</td>
    </tr>
  </tbody>
</table>
<p>1996 rows × 6 columns</p>
</div>




```python
len(df.title.unique())
```




    1992




```python
df.shape
```




    (1996, 6)




```python
df.dtypes
```




    title      object
    revenue    object
    budget     object
    genres     object
    year       object
    date       object
    dtype: object



#### Convert 'budget' and 'revenue' columns to numeric data


```python
df['revenue'] = pd.to_numeric(df.revenue)
df['budget'] = pd.to_numeric(df.budget)
```


```python
df.dtypes
```




    title      object
    revenue     int64
    budget      int64
    genres     object
    year       object
    date       object
    dtype: object



#### Filter out films with revenue under $1M


```python
df1 = df[df.revenue.gt(1000000)]
```

#### Explode 'genres' column so we can compare single genres


```python
genre_df = df1.explode('genres')
genre_df.shape
```




    (5130, 6)




```python
genre_df.groupby(['year', 'genres']).mean().sort_values(['year', 'revenue'], ascending=False).head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>revenue</th>
      <th>budget</th>
    </tr>
    <tr>
      <th>year</th>
      <th>genres</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="17" valign="top">2019</td>
      <td>Science Fiction</td>
      <td>4.924485e+08</td>
      <td>1.206176e+08</td>
    </tr>
    <tr>
      <td>Adventure</td>
      <td>4.606954e+08</td>
      <td>1.115714e+08</td>
    </tr>
    <tr>
      <td>Family</td>
      <td>2.808875e+08</td>
      <td>7.131111e+07</td>
    </tr>
    <tr>
      <td>Animation</td>
      <td>2.340574e+08</td>
      <td>4.666365e+07</td>
    </tr>
    <tr>
      <td>Action</td>
      <td>2.185427e+08</td>
      <td>6.123766e+07</td>
    </tr>
    <tr>
      <td>Fantasy</td>
      <td>2.167856e+08</td>
      <td>6.039139e+07</td>
    </tr>
    <tr>
      <td>Mystery</td>
      <td>1.133265e+08</td>
      <td>1.479130e+07</td>
    </tr>
    <tr>
      <td>Thriller</td>
      <td>1.109072e+08</td>
      <td>2.269645e+07</td>
    </tr>
    <tr>
      <td>Comedy</td>
      <td>1.103256e+08</td>
      <td>2.805747e+07</td>
    </tr>
    <tr>
      <td>Crime</td>
      <td>1.047132e+08</td>
      <td>2.606880e+07</td>
    </tr>
    <tr>
      <td>Horror</td>
      <td>8.695294e+07</td>
      <td>1.602273e+07</td>
    </tr>
    <tr>
      <td>Romance</td>
      <td>7.874462e+07</td>
      <td>1.698843e+07</td>
    </tr>
    <tr>
      <td>Drama</td>
      <td>6.965347e+07</td>
      <td>1.901188e+07</td>
    </tr>
    <tr>
      <td>War</td>
      <td>6.822902e+07</td>
      <td>3.057450e+07</td>
    </tr>
    <tr>
      <td>History</td>
      <td>6.333776e+07</td>
      <td>3.659754e+07</td>
    </tr>
    <tr>
      <td>Music</td>
      <td>5.344923e+07</td>
      <td>1.946381e+07</td>
    </tr>
    <tr>
      <td>Documentary</td>
      <td>2.948430e+06</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2018</td>
      <td>Adventure</td>
      <td>3.737049e+08</td>
      <td>9.336125e+07</td>
    </tr>
    <tr>
      <td>Science Fiction</td>
      <td>3.617935e+08</td>
      <td>8.530000e+07</td>
    </tr>
    <tr>
      <td>Action</td>
      <td>3.064245e+08</td>
      <td>6.247300e+07</td>
    </tr>
  </tbody>
</table>
</div>



#### Create lists of genres for our plot


```python
gen1 = ['Science Fiction', 'Adventure', 'Family']
gen2 = ['Animation', 'Action', 'Fantasy']
gen3 = ['Mystery', 'Thriller', 'Comedy']
gen4 = ['Crime', 'Horror', 'Romance']
gen5 = ['Drama', 'War', 'History']
gen6 = ['Music', 'Documentary', 'Western']
```


```python
gen1_df = genre_df[genre_df.genres.isin(gen1)]
gen2_df = genre_df[genre_df.genres.isin(gen2)]
gen3_df = genre_df[genre_df.genres.isin(gen3)]
gen4_df = genre_df[genre_df.genres.isin(gen4)]
gen5_df = genre_df[genre_df.genres.isin(gen5)]
gen6_df = genre_df[genre_df.genres.isin(gen6)]
```

#### Plot average revenue by genre as compared to average revenue overall


```python
import seaborn as sns
fig, (ax1,ax2,ax3) = plt.subplots(3, 2, figsize=(15,10), sharex=True, sharey=True)

sns.lineplot(x='year', y='revenue', data=gen1_df, hue='genres', ax=ax1[0])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax1[0], color='black')
ax1[0].set_title('Family, Adventure, Comedy')

sns.lineplot(x='year', y='revenue', data=gen2_df, hue='genres', ax=ax1[1])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax1[1], color='black')
ax1[1].set_title('Animation, Fantasy, Action')

sns.lineplot(x='year', y='revenue', data=gen3_df, hue='genres', ax=ax2[0])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax2[0], color='black')
ax2[0].set_title('Comedy, Thriller, Mystery')

sns.lineplot(x='year', y='revenue', data=gen4_df, hue='genres', ax=ax2[1])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax2[1], color='black')
ax2[1].set_title('Romance, Horror, Crime')

sns.lineplot(x='year', y='revenue', data=gen5_df, hue='genres', ax=ax3[0])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax3[0], color='black')
ax3[0].set_title('Drama, History, War')

sns.lineplot(x='year', y='revenue', data=gen6_df, hue='genres', ax=ax3[1])
sns.lineplot(data=genre_df.groupby('year').revenue.mean(), ax=ax3[1], color='black')
ax3[1].set_title('Western, Documentary, Music')

fig.suptitle("Average Revenue by Genre Compared to Overall Average")
ax3[0].set_xlabel("Year")
ax3[1].set_xlabel("Year")
ax1[0].set_ylabel("Revenue (100M)")
ax2[0].set_ylabel("Revenue (100M)")
ax3[0].set_ylabel("Revenue (100M)")
```




    Text(0, 0.5, 'Revenue (100M)')




![png](output_29_1.png)


##### Here we can see that Family, Adventure, Sci-Fi, Animation, Fantasy, and Action all regularly perform better than average. Westerns, Documentaries, Music Movies, and War Movies are hit-or-miss from year-to-year, and the remaining genres regulary perform below the average.

#### Gathering information specific to 2019


```python
year_19_df = genre_df[genre_df['year']=='2019'].sort_values('revenue', ascending=False)
year_19_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1796</td>
      <td>Avengers: Endgame</td>
      <td>2797800564</td>
      <td>356000000</td>
      <td>Adventure</td>
      <td>2019</td>
      <td>04-24</td>
    </tr>
    <tr>
      <td>1796</td>
      <td>Avengers: Endgame</td>
      <td>2797800564</td>
      <td>356000000</td>
      <td>Action</td>
      <td>2019</td>
      <td>04-24</td>
    </tr>
    <tr>
      <td>1796</td>
      <td>Avengers: Endgame</td>
      <td>2797800564</td>
      <td>356000000</td>
      <td>Science Fiction</td>
      <td>2019</td>
      <td>04-24</td>
    </tr>
    <tr>
      <td>1797</td>
      <td>The Lion King</td>
      <td>1656943394</td>
      <td>260000000</td>
      <td>Adventure</td>
      <td>2019</td>
      <td>07-12</td>
    </tr>
    <tr>
      <td>1797</td>
      <td>The Lion King</td>
      <td>1656943394</td>
      <td>260000000</td>
      <td>Family</td>
      <td>2019</td>
      <td>07-12</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>1994</td>
      <td>Chhalawa</td>
      <td>1300000</td>
      <td>0</td>
      <td>Drama</td>
      <td>2019</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>1994</td>
      <td>Chhalawa</td>
      <td>1300000</td>
      <td>0</td>
      <td>Family</td>
      <td>2019</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>1994</td>
      <td>Chhalawa</td>
      <td>1300000</td>
      <td>0</td>
      <td>Romance</td>
      <td>2019</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>1995</td>
      <td>Roger Waters: Us + Them</td>
      <td>1294480</td>
      <td>0</td>
      <td>Documentary</td>
      <td>2019</td>
      <td>10-02</td>
    </tr>
    <tr>
      <td>1995</td>
      <td>Roger Waters: Us + Them</td>
      <td>1294480</td>
      <td>0</td>
      <td>Music</td>
      <td>2019</td>
      <td>10-02</td>
    </tr>
  </tbody>
</table>
<p>504 rows × 6 columns</p>
</div>




```python
sns.barplot(orient='h', data=year_19_df, x='revenue', y='genres')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2aebb5b96d8>




![png](output_33_1.png)


#### Gathering information specific to 2020


```python
year_20_df = pd.DataFrame(columns=columns)
page = 1
while page <= 10:
    url = 'https://api.themoviedb.org/3/discover/movie?api_key=' + api_key + \
      '&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false' + \
      '&primary_release_year=2020&page=' + str(page)
    print(url)
    year_2020 = requests.get(url)
    year_2020 = year_2020.json()
    results = year_2020['results']
    page += 1
    for film in results:
        print(film['title'])
        try:
            film_rev = requests.get('https://api.themoviedb.org/3/movie/' + str(film['id']) +\
                                    '?api_key=' + api_key + '&language=en-US')
            film_rev = film_rev.json()
            details = [film['title'], film_rev['revenue'], film_rev['budget'],\
                       [x['name'] for x in film_rev['genres']], film_rev['release_date'][:4],\
                       film_rev['release_date'][5:]]
            year_20_df.loc[len(year_20_df)]=details
        except:
            continue
```

    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=1
    Bad Boys for Life
    Sonic the Hedgehog
    Dolittle
    Birds of Prey (and the Fantabulous Emancipation of One Harley Quinn)
    The Invisible Man
    The Gentlemen
    The Call of the Wild
    Onward
    Scarlet Tulips
    Fantasy Island
    The Grudge
    Underwater
    Invasion
    Ala Vaikunthapurramuloo
    Bloodshot
    Like a Boss
    Emma.
    Gretel & Hansel
    Capone
    Brahms: The Boy II
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=2
    The Turning
    The Way Back
    Street Dancer 3D
    Malang
    UFC 246: McGregor vs. Cowboy
    Impractical Jokers: The Movie
    Nightlife
    Downhill
    The Hunt
    My Spy
    Varane Avashyamund
    Lassie Come Home
    Trolls World Tour
    The Perfect Date
    Block Z
    The Elfkins - Baking a Difference
    Sudakshinar Saree
    I Love You, Stupid
    ALL COPS AIN'T BAD
    Maceracı Yüzgeçler: Büyük Gösteri
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=3
    The Mafia: Payback
    Enter the Fat Dragon
    Permette? Alberto Sordi
    Boundary
    Tokoloshe: An African Curse
    The Passenger
    Kabaddi Kabaddi Kabaddi
    Resistance
    Sí, Mi Amor
    Moondance
    Brothers
    Detective Dee : Deep Sea Dragon Palace
    Batman: Most Wanted
    Mothman
    Involuntary Solitude
    Imitation
    One Day
    Glop
    Carpe Diem
    Live Periandroy me Stefo
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=4
    The Everyday
    El Filibusterismo
    Four Of A Kind
    Stalking Ian:The New Cut
    La mort d'Emi o (Un voyage de découverte de soi)
    Wander
    Bottle
    Duck
    Rhythm, Office, Sub
    God Mode.exe
    Passage of Time
    RGC
    Current Sea
    Kim Il Sung's Children
    Ming Ri Chong Sheng
    We are the Champions - 50 Jahre Queen
    Just Before I Leave
    Millennium Hour
    When I’m at Home
    Maralinga Tjarutja
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=5
    The Circadian Cycle
    Evolver
    Turner Risk
    Hidden Secrets
    大神猴3情劫篇
    Alejandro & Miguel
    The Convergence of Souls
    Waves (2020)
    Auguste Escoffier: The Birth of Haute Cuisine
    Goodbye Friend
    Manifest
    Nightmares
    Ainda Existe Amor
    Sqizo
    Quero Ser Helena
    Body Double 37
    A Story That Chris Told Me
    Madu
    Pętla
    White Flowers
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=6
    Icaro I
    Lostceivers
    Horrid Henry's Wild Weekend
    Ada Apa Dengan Dosa
    Multi-Dancer
    Journal
    Woohoo! I'm A Pilot!
    Could This Be The One?
    Go Away
    Keu-ri-seu-ma-seu
    Catch Ball
    8mm
    Big Breasts Sister 2
    Master Zhang
    The Red Marsh
    The Cursed City Filled With Blood
    Only the Brave: A New Musical
    Una Situación Extraordinaria
    Alfred
    Les Joueuses #Paslàpourdanser
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=7
    Vita Vera - Story
    Le royaume de la lumière
    The Wall of Shadows
    Armstrong
    Metallica: Live in Salt Lake City, Utah - January 2, 1997
    Удельный
    Bajo mi piel morena
    Jaws — Assembling a Top-Tier Team (feat. TierZoo)
    Spitzenmedizin: Akupunktur - Mythos oder Therapie?
    Faites qu'il nous arrive quelque chose
    Bubble Dum
    Boo! Appetweet
    La Fille oblique
    We Are George Floyd
    It's Okay to Panic
    Fútbol y racismo (Los Otros)
    The Lsat Rafter
    Letters to Eloisa
    Adam Bloom
    Porque a Escuridão é Necessária para Experimentar a Plenitude da Luz
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=8
    Seven Menus
    Lai Lag
    My Senior Year
    Six Weeks to Twelve Years
    Gina Brillon: The Floor Is Lava
    Rizo
    As We Are
    Change: Expanding the Concept of Justice in America
    Before and After Detention
    Via Crucis
    Hasta domingo soy gringo
    The Mistress
    Cover/Age
    Sunshine Room
    The Cuphead Show !
    xABo: Father Boniecki
    Stolen
    Le Sommet des dieux
    Baba Yaga
    Vanille
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=9
    Miami City Ballet's The Firebird
    Pollywood
    Final Exam
    La Petite taupe aime la nature
    Neighbor at Home
    Welcome Strangers
    Susana
    styx
    The City Bridges Are Open Again
    A Youth
    Cinéma par... Cédric Klapisch
    DARK Fan Film
    Engel
    Thanks & Sorry
    Desolation
    Fixed
    War Hero
    Sleazy Does It With Sally Mullins
    Family visits the Sugarloaf Mountain
    Luz
    https://api.themoviedb.org/3/discover/movie?api_key=37d93a47466b976ea47e8585c7bdace0&language=en-US&sort_by=revenue.desc&include_adult=false&include_video=false&primary_release_year=2020&page=10
    Little Miss Fate
    Powderfinger One Night Lonely
    Coś sędzia długą przerwę zarządził
    Remembrance of József Romvári
    Childbirth Banned in Paradise
    We Have all the Time in the World
    This Land
    Katabasis
    All the Things She Said
    Lakomec
    Mangrove
    Lovers Rock
    Wings for Coldplay
    In There
    light of temptation
    The Big Hit
    Say My Name
    Birthday Boy
    16 Springs
    Jamie Kennedy: Stoopid Smart
    


```python
year_20_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Bad Boys for Life</td>
      <td>191150000</td>
      <td>0</td>
      <td>[Thriller, Action, Crime]</td>
      <td>2020</td>
      <td>01-15</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Sonic the Hedgehog</td>
      <td>306766470</td>
      <td>85000000</td>
      <td>[Action, Science Fiction, Comedy, Family]</td>
      <td>2020</td>
      <td>02-12</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Dolittle</td>
      <td>223343452</td>
      <td>175000000</td>
      <td>[Comedy, Fantasy, Adventure, Family]</td>
      <td>2020</td>
      <td>01-01</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Birds of Prey (and the Fantabulous Emancipatio...</td>
      <td>201858461</td>
      <td>75000000</td>
      <td>[Action, Crime, Comedy]</td>
      <td>2020</td>
      <td>02-05</td>
    </tr>
    <tr>
      <td>4</td>
      <td>The Invisible Man</td>
      <td>123414678</td>
      <td>9000000</td>
      <td>[Thriller, Science Fiction, Horror]</td>
      <td>2020</td>
      <td>02-26</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>194</td>
      <td>The Big Hit</td>
      <td>0</td>
      <td>0</td>
      <td>[Comedy, Drama]</td>
      <td>2020</td>
      <td>10-28</td>
    </tr>
    <tr>
      <td>195</td>
      <td>Say My Name</td>
      <td>0</td>
      <td>0</td>
      <td>[Documentary]</td>
      <td>2020</td>
      <td>05-12</td>
    </tr>
    <tr>
      <td>196</td>
      <td>Birthday Boy</td>
      <td>0</td>
      <td>0</td>
      <td>[Drama]</td>
      <td>2020</td>
      <td>06-05</td>
    </tr>
    <tr>
      <td>197</td>
      <td>16 Springs</td>
      <td>0</td>
      <td>0</td>
      <td>[]</td>
      <td>2020</td>
      <td>12-09</td>
    </tr>
    <tr>
      <td>198</td>
      <td>Jamie Kennedy: Stoopid Smart</td>
      <td>0</td>
      <td>0</td>
      <td>[Comedy]</td>
      <td>2020</td>
      <td>05-25</td>
    </tr>
  </tbody>
</table>
<p>199 rows × 6 columns</p>
</div>




```python
year_20_df.dtypes
```




    title      object
    revenue    object
    budget     object
    genres     object
    year       object
    date       object
    dtype: object



#### Make 'budget' and 'revenue' numeric again


```python
year_20_df['revenue'] = pd.to_numeric(year_20_df.revenue)
year_20_df['budget'] = pd.to_numeric(year_20_df.budget)
```


```python
year_20_df.dtypes
```




    title      object
    revenue     int64
    budget      int64
    genres     object
    year       object
    date       object
    dtype: object



#### Only use films with revenue over 1M to keep data comparable to previous years


```python
year_20_df = year_20_df[year_20_df.revenue.gt(1000000)]
```


```python
year_20_df = year_20_df.explode('genres')
```


```python
year_20_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Bad Boys for Life</td>
      <td>191150000</td>
      <td>0</td>
      <td>Thriller</td>
      <td>2020</td>
      <td>01-15</td>
    </tr>
    <tr>
      <td>0</td>
      <td>Bad Boys for Life</td>
      <td>191150000</td>
      <td>0</td>
      <td>Action</td>
      <td>2020</td>
      <td>01-15</td>
    </tr>
    <tr>
      <td>0</td>
      <td>Bad Boys for Life</td>
      <td>191150000</td>
      <td>0</td>
      <td>Crime</td>
      <td>2020</td>
      <td>01-15</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Sonic the Hedgehog</td>
      <td>306766470</td>
      <td>85000000</td>
      <td>Action</td>
      <td>2020</td>
      <td>02-12</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Sonic the Hedgehog</td>
      <td>306766470</td>
      <td>85000000</td>
      <td>Science Fiction</td>
      <td>2020</td>
      <td>02-12</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Trolls World Tour</td>
      <td>1946164</td>
      <td>0</td>
      <td>Family</td>
      <td>2020</td>
      <td>03-12</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Trolls World Tour</td>
      <td>1946164</td>
      <td>0</td>
      <td>Comedy</td>
      <td>2020</td>
      <td>03-12</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Trolls World Tour</td>
      <td>1946164</td>
      <td>0</td>
      <td>Fantasy</td>
      <td>2020</td>
      <td>03-12</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Trolls World Tour</td>
      <td>1946164</td>
      <td>0</td>
      <td>Adventure</td>
      <td>2020</td>
      <td>03-12</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Trolls World Tour</td>
      <td>1946164</td>
      <td>0</td>
      <td>Music</td>
      <td>2020</td>
      <td>03-12</td>
    </tr>
  </tbody>
</table>
<p>91 rows × 6 columns</p>
</div>




```python
sns.barplot(orient='h', data=year_20_df, x='revenue', y='genres')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2aebb6904a8>




![png](output_45_1.png)


#### Plot to compare average revenue by genre from 2019 to 2020 so far


```python
fig, ax1 = plt.subplots(1, 2, figsize=(15,10), sharey=True)
sns.barplot(orient='h', data=year_19_df, x='revenue', y='genres', ax=ax1[0])
ax1[0].set_title("2019")
ax1[0].set_xlabel("Revenue (100M)")
ax1[0].set_ylabel('Genres')
sns.barplot(orient='h', data=year_20_df, x='revenue', y='genres', ax=ax1[1])
sns.pointplot(data=year_19_df, x='revenue', y='genres', ax=ax1[1])
ax1[1].set_title('2020 So Far vs. 2019')
ax1[1].set_xlabel('Revenue (100M)')
ax1[1].set_ylabel("")
fig.suptitle("Average Revenues by Genre in 2019, 2020")
```




    Text(0.5, 0.98, 'Average Revenues by Genre in 2019, 2020')




![png](output_47_1.png)


## How does a film's budget affect its performance?

The file 'tn.movie_budgets.csv.gz' contains all the needed information for this, so we just put it into a dataframe and make a simple regression plot.


```python
fig, ax = plt.subplots(figsize=(12,7))
sns.regplot(data=df, x='budget', y='revenue')
ax.set_title('Budget vs. Revenue')
ax.set_xlabel('Budget (100M)')
ax.set_ylabel('Revenue (1B)')
```




    Text(0, 0.5, 'Revenue (1B)')




![png](output_49_1.png)


##### This shows a strong relationship between a film's budget and its revenue. Not surprisingly, the more money invested in a film, the more you can expect in revenue, as a general rule.

## What is the effect of release date on expected revenue?


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>1066969703</td>
      <td>200000000</td>
      <td>[Animation, Family, Comedy]</td>
      <td>2010</td>
      <td>06-16</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland</td>
      <td>1025467110</td>
      <td>200000000</td>
      <td>[Family, Fantasy, Adventure]</td>
      <td>2010</td>
      <td>03-03</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>954305868</td>
      <td>250000000</td>
      <td>[Adventure, Fantasy]</td>
      <td>2010</td>
      <td>10-17</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>825532764</td>
      <td>160000000</td>
      <td>[Action, Science Fiction, Adventure]</td>
      <td>2010</td>
      <td>07-15</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>752600867</td>
      <td>165000000</td>
      <td>[Comedy, Adventure, Fantasy, Animation, Family]</td>
      <td>2010</td>
      <td>05-16</td>
    </tr>
  </tbody>
</table>
</div>



#### Getting the month from the date column


```python
df['month'] = [[x][0][:2] for x in df['date']]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>1066969703</td>
      <td>200000000</td>
      <td>[Animation, Family, Comedy]</td>
      <td>2010</td>
      <td>06-16</td>
      <td>06</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland</td>
      <td>1025467110</td>
      <td>200000000</td>
      <td>[Family, Fantasy, Adventure]</td>
      <td>2010</td>
      <td>03-03</td>
      <td>03</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>954305868</td>
      <td>250000000</td>
      <td>[Adventure, Fantasy]</td>
      <td>2010</td>
      <td>10-17</td>
      <td>10</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>825532764</td>
      <td>160000000</td>
      <td>[Action, Science Fiction, Adventure]</td>
      <td>2010</td>
      <td>07-15</td>
      <td>07</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>752600867</td>
      <td>165000000</td>
      <td>[Comedy, Adventure, Fantasy, Animation, Family]</td>
      <td>2010</td>
      <td>05-16</td>
      <td>05</td>
    </tr>
  </tbody>
</table>
</div>



#### Using month to determine quarter


```python
import math
df['quarter'] = [math.ceil(int(x)/3) for x in df['month']]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
      <th>month</th>
      <th>quarter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Toy Story 3</td>
      <td>1066969703</td>
      <td>200000000</td>
      <td>[Animation, Family, Comedy]</td>
      <td>2010</td>
      <td>06-16</td>
      <td>06</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Alice in Wonderland</td>
      <td>1025467110</td>
      <td>200000000</td>
      <td>[Family, Fantasy, Adventure]</td>
      <td>2010</td>
      <td>03-03</td>
      <td>03</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>954305868</td>
      <td>250000000</td>
      <td>[Adventure, Fantasy]</td>
      <td>2010</td>
      <td>10-17</td>
      <td>10</td>
      <td>4</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Inception</td>
      <td>825532764</td>
      <td>160000000</td>
      <td>[Action, Science Fiction, Adventure]</td>
      <td>2010</td>
      <td>07-15</td>
      <td>07</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Shrek Forever After</td>
      <td>752600867</td>
      <td>165000000</td>
      <td>[Comedy, Adventure, Fantasy, Animation, Family]</td>
      <td>2010</td>
      <td>05-16</td>
      <td>05</td>
      <td>2</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>1991</td>
      <td>One Love , Two Lives</td>
      <td>1401422</td>
      <td>0</td>
      <td>[Drama, Romance]</td>
      <td>2019</td>
      <td>02-15</td>
      <td>02</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1992</td>
      <td>Wrong No. 2</td>
      <td>1400000</td>
      <td>0</td>
      <td>[Romance, Comedy]</td>
      <td>2019</td>
      <td>06-05</td>
      <td>06</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1993</td>
      <td>El Chicano</td>
      <td>1370000</td>
      <td>0</td>
      <td>[Drama, Action, Crime]</td>
      <td>2019</td>
      <td>05-03</td>
      <td>05</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1994</td>
      <td>Chhalawa</td>
      <td>1300000</td>
      <td>0</td>
      <td>[Comedy, Drama, Family, Romance]</td>
      <td>2019</td>
      <td>06-05</td>
      <td>06</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1995</td>
      <td>Roger Waters: Us + Them</td>
      <td>1294480</td>
      <td>0</td>
      <td>[Documentary, Music]</td>
      <td>2019</td>
      <td>10-02</td>
      <td>10</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>1996 rows × 8 columns</p>
</div>



#### Plot by quarter for 2010-2019


```python
sns.lineplot(data=df, x='year', y='revenue', hue='quarter', palette='Dark2')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2aec005e860>




![png](output_58_1.png)


#### find release quarters for 2020


```python
year_20_df['month'] = [[x][0][:2] for x in year_20_df['date']]
year_20_df['quarter'] = [math.ceil(int(x)/3) for x in year_20_df['month']]
year_20_df.groupby('quarter').count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>revenue</th>
      <th>budget</th>
      <th>genres</th>
      <th>year</th>
      <th>date</th>
      <th>month</th>
    </tr>
    <tr>
      <th>quarter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>89</td>
      <td>89</td>
      <td>89</td>
      <td>88</td>
      <td>89</td>
      <td>89</td>
      <td>89</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



#### Plot 2020 by month since there are only 2 quarters to show


```python
sns.lineplot(data=year_20_df, x='quarter', y='revenue')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2aec1917c18>




![png](output_62_1.png)


#### Show plot comparing past years quarterly to this year


```python
fig, axes = plt.subplots(1, 2, figsize=(15,5), sharey=True)
sns.lineplot(data=df, x='year', y='revenue', hue='quarter', palette='Dark2', ax=axes[0])
sns.lineplot(data=year_20_df, x='month', y='revenue', ax=axes[1])
axes[0].set_title('Revenue by Quarter 2010-2019')
axes[0].set_xlabel('Year')
axes[0].set_ylabel('Revenue (100M)')
axes[1].set_title('Revenue by Month 2020')
axes[1].set_xlabel('Month')
fig.suptitle("A Comparison of Revenue by Release Schedule")
```




    Text(0.5, 0.98, 'A Comparison of Revenue by Release Schedule')




![png](output_64_1.png)


##### Here we can see that we can expect a bump in revenue in quarters 2 and 4. So, the best time to release our film is in the summer or during the holiday season. 2020 has not gotten its quarter 2 bump, presumably due to Covid-19 restrictions.

### Recommendation: Big budget Sci-Fi/Adventure film to release in summer or the holiday season. Revenues are currently below expectations due to Covid-19, but we can expect that slump to be over before the film finishes production.
