
---
title: Blogging from a Python Jupyter Notebook
author: Marlan
date: 2018-03-30
categories:
  - Python
tags:
  - Blogdown
  - Python
output:
  blogdown::html_page:
    toc: true
---

This is a follow on post to Stefan's [original](/posts/how_to_add_a_blog/) to show how to generate a blog post from a Jupyter Notebook instead of an R markdown. This post itself started off life as a Jupyter Notebook which lives in the same `content/posts` folder as the other Rmd files used for the site. We'll walk through how it became a blog post.

The process is a little more complicated than for the Rmd files (since that's what Blogdown was built for but we can still get it to work relatively easily. The process will involve setting up the Jupyter Notebook as usual, converting to a standard Markdown file, and then making some minor edits to make sure everything displays correctly.

## Step 0: Start off with the header block

The first thing you'll notice (if you're looking at the original notebook file) is the Rmd style header block at the top of the notebook. This would not generally be part of a Jupyter Notebook or a regular Markdown file, but Blogdown requires it so that it knows what to do with this post we I just copy-pasted one from another post and edited it accordingly.

For this post it looks like
```
---
title: Blogging from a Python Jupyter Notebook
author: Marlan
date: 2018-03-30
categories:
  - Python
tags:
  - Blogdown
  - Python
output:
  blogdown::html_page:
    toc: true
---
```

## Step 1: Prepare a Jupyter Notebook with some useful content

### Basic Formatting

We'll start off with some text.

You can use all the usual markdown stuff like **bold** text and *italics*, 

quotes
> Beautiful is better than ugly.  
Explicit is better than implicit.  
Simple is better than complex.  
Complex is better than complicated.  

and lists

- [x] Write a blog on writing blogs
- [ ] Write a blog that's not quite so meta
- [ ] Do some actual analytics

Jupyter uses the same flavour of markdown as Github so you can refer to the reference [here](https://help.github.com/articles/basic-writing-and-formatting-syntax/) for more complicated formatting

### Images

![python meme](../../static/Pictures/python_meme.jpg)

Place any images you want to include in the `static/Pictures` folder (create a specific folder in there for your post to keep things neat).

The syntax to insert an image is the same as in the Rmd. For example, for the image above I used
```md
![python meme](../../static/Pictures/python_meme.jpg)
```

Note that I used the relative path to the image from the `content/Posts` folder so that the image displays correctly in the Jupyter Notebook. If you used this as is in Blogdown, it wouldn't work (you'd see a broken image link with the alt text *python meme*). 

Paths are a bit tricksy in Blogdown. It considers the `static` folder to be the root so we'll have to change this in the final markdown to
```md
![python meme](/Pictures/python_meme.jpg)
```

### Code

Of course the whole point of doing this in a Jupyter Notebook is to actually write and run some code (otherwise we could just write this in plain Markdown)

Let's get some interesting data from a public api and do a quick plot. I'm going to track driver Fernando Alonso's travails during the 2017 Formula 1 season, grabbing data from this [site](https://ergast.com/mrd/). 

We'll use the `requests` library to make the HTTP GET request to the API


```python
import requests
import json
```


```python
alonso_results = requests.get("https://ergast.com/api/f1/2017/drivers/alonso/results.json").json()
print(json.dumps(alonso_results, indent=2))
```

    {
      "MRData": {
        "xmlns": "http://ergast.com/mrd/1.4",
        "series": "f1",
        "url": "http://ergast.com/api/f1/2017/drivers/alonso/results.json",
        "limit": "30",
        "offset": "0",
        "total": "19",
        "RaceTable": {
          "season": "2017",
          "driverId": "alonso",
          "Races": [
            {
              "season": "2017",
              "round": "1",
              "url": "https://en.wikipedia.org/wiki/2017_Australian_Grand_Prix",
              "raceName": "Australian Grand Prix",
              "Circuit": {
                "circuitId": "albert_park",
                "url": "http://en.wikipedia.org/wiki/Melbourne_Grand_Prix_Circuit",
                "circuitName": "Albert Park Grand Prix Circuit",
                "Location": {
                  "lat": "-37.8497",
                  "long": "144.968",
                  "locality": "Melbourne",
                  "country": "Australia"
                }
              },
              "date": "2017-03-26",
              "time": "05:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "14",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "12",
                  "laps": "50",
                  "status": "Vibrations",
                  "FastestLap": {
                    "rank": "17",
                    "lap": "48",
                    "Time": {
                      "time": "1:30.077"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "211.938"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "2",
              "url": "https://en.wikipedia.org/wiki/2017_Chinese_Grand_Prix",
              "raceName": "Chinese Grand Prix",
              "Circuit": {
                "circuitId": "shanghai",
                "url": "http://en.wikipedia.org/wiki/Shanghai_International_Circuit",
                "circuitName": "Shanghai International Circuit",
                "Location": {
                  "lat": "31.3389",
                  "long": "121.22",
                  "locality": "Shanghai",
                  "country": "China"
                }
              },
              "date": "2017-04-09",
              "time": "06:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "16",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "13",
                  "laps": "33",
                  "status": "Driveshaft",
                  "FastestLap": {
                    "rank": "15",
                    "lap": "31",
                    "Time": {
                      "time": "1:39.496"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "197.230"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "3",
              "url": "https://en.wikipedia.org/wiki/2017_Bahrain_Grand_Prix",
              "raceName": "Bahrain Grand Prix",
              "Circuit": {
                "circuitId": "bahrain",
                "url": "http://en.wikipedia.org/wiki/Bahrain_International_Circuit",
                "circuitName": "Bahrain International Circuit",
                "Location": {
                  "lat": "26.0325",
                  "long": "50.5106",
                  "locality": "Sakhir",
                  "country": "Bahrain"
                }
              },
              "date": "2017-04-16",
              "time": "15:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "14",
                  "positionText": "14",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "15",
                  "laps": "54",
                  "status": "Power Unit",
                  "FastestLap": {
                    "rank": "14",
                    "lap": "47",
                    "Time": {
                      "time": "1:35.595"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "203.809"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "4",
              "url": "https://en.wikipedia.org/wiki/2017_Russian_Grand_Prix",
              "raceName": "Russian Grand Prix",
              "Circuit": {
                "circuitId": "sochi",
                "url": "http://en.wikipedia.org/wiki/Sochi_Autodrom",
                "circuitName": "Sochi Autodrom",
                "Location": {
                  "lat": "43.4057",
                  "long": "39.9578",
                  "locality": "Sochi",
                  "country": "Russia"
                }
              },
              "date": "2017-04-30",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "20",
                  "positionText": "W",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "15",
                  "laps": "0",
                  "status": "Power Unit"
                }
              ]
            },
            {
              "season": "2017",
              "round": "5",
              "url": "https://en.wikipedia.org/wiki/2017_Spanish_Grand_Prix",
              "raceName": "Spanish Grand Prix",
              "Circuit": {
                "circuitId": "catalunya",
                "url": "http://en.wikipedia.org/wiki/Circuit_de_Barcelona-Catalunya",
                "circuitName": "Circuit de Barcelona-Catalunya",
                "Location": {
                  "lat": "41.57",
                  "long": "2.26111",
                  "locality": "Montmel\u00f3",
                  "country": "Spain"
                }
              },
              "date": "2017-05-14",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "12",
                  "positionText": "12",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "7",
                  "laps": "64",
                  "status": "+2 Laps",
                  "FastestLap": {
                    "rank": "4",
                    "lap": "64",
                    "Time": {
                      "time": "1:23.894"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "199.752"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "7",
              "url": "https://en.wikipedia.org/wiki/2017_Canadian_Grand_Prix",
              "raceName": "Canadian Grand Prix",
              "Circuit": {
                "circuitId": "villeneuve",
                "url": "http://en.wikipedia.org/wiki/Circuit_Gilles_Villeneuve",
                "circuitName": "Circuit Gilles Villeneuve",
                "Location": {
                  "lat": "45.5",
                  "long": "-73.5228",
                  "locality": "Montreal",
                  "country": "Canada"
                }
              },
              "date": "2017-06-11",
              "time": "18:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "16",
                  "positionText": "16",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "12",
                  "laps": "66",
                  "status": "Power Unit",
                  "FastestLap": {
                    "rank": "4",
                    "lap": "63",
                    "Time": {
                      "time": "1:15.853"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "206.974"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "8",
              "url": "https://en.wikipedia.org/wiki/2017_Azerbaijan_Grand_Prix",
              "raceName": "Azerbaijan Grand Prix",
              "Circuit": {
                "circuitId": "BAK",
                "url": "http://en.wikipedia.org/wiki/Baku_City_Circuit",
                "circuitName": "Baku City Circuit",
                "Location": {
                  "lat": "40.3725",
                  "long": "49.8533",
                  "locality": "Baku",
                  "country": "Azerbaijan"
                }
              },
              "date": "2017-06-25",
              "time": "13:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "9",
                  "positionText": "9",
                  "points": "2",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "19",
                  "laps": "51",
                  "status": "Finished",
                  "Time": {
                    "millis": "7495124",
                    "time": "+59.551"
                  },
                  "FastestLap": {
                    "rank": "6",
                    "lap": "49",
                    "Time": {
                      "time": "1:45.168"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "205.488"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "9",
              "url": "https://en.wikipedia.org/wiki/2017_Austrian_Grand_Prix",
              "raceName": "Austrian Grand Prix",
              "Circuit": {
                "circuitId": "red_bull_ring",
                "url": "http://en.wikipedia.org/wiki/Red_Bull_Ring",
                "circuitName": "Red Bull Ring",
                "Location": {
                  "lat": "47.2197",
                  "long": "14.7647",
                  "locality": "Spielburg",
                  "country": "Austria"
                }
              },
              "date": "2017-07-09",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "19",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "12",
                  "laps": "1",
                  "status": "Collision damage"
                }
              ]
            },
            {
              "season": "2017",
              "round": "10",
              "url": "https://en.wikipedia.org/wiki/2017_British_Grand_Prix",
              "raceName": "British Grand Prix",
              "Circuit": {
                "circuitId": "silverstone",
                "url": "http://en.wikipedia.org/wiki/Silverstone_Circuit",
                "circuitName": "Silverstone Circuit",
                "Location": {
                  "lat": "52.0786",
                  "long": "-1.01694",
                  "locality": "Silverstone",
                  "country": "UK"
                }
              },
              "date": "2017-07-16",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "18",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "20",
                  "laps": "32",
                  "status": "Power Unit",
                  "FastestLap": {
                    "rank": "18",
                    "lap": "22",
                    "Time": {
                      "time": "1:34.263"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "224.983"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "11",
              "url": "https://en.wikipedia.org/wiki/2017_Hungarian_Grand_Prix",
              "raceName": "Hungarian Grand Prix",
              "Circuit": {
                "circuitId": "hungaroring",
                "url": "http://en.wikipedia.org/wiki/Hungaroring",
                "circuitName": "Hungaroring",
                "Location": {
                  "lat": "47.5789",
                  "long": "19.2486",
                  "locality": "Budapest",
                  "country": "Hungary"
                }
              },
              "date": "2017-07-30",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "6",
                  "positionText": "6",
                  "points": "8",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "7",
                  "laps": "70",
                  "status": "Finished",
                  "Time": {
                    "millis": "6057936",
                    "time": "+1:11.223"
                  },
                  "FastestLap": {
                    "rank": "1",
                    "lap": "69",
                    "Time": {
                      "time": "1:20.182"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "196.697"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "12",
              "url": "https://en.wikipedia.org/wiki/2017_Belgian_Grand_Prix",
              "raceName": "Belgian Grand Prix",
              "Circuit": {
                "circuitId": "spa",
                "url": "http://en.wikipedia.org/wiki/Circuit_de_Spa-Francorchamps",
                "circuitName": "Circuit de Spa-Francorchamps",
                "Location": {
                  "lat": "50.4372",
                  "long": "5.97139",
                  "locality": "Spa",
                  "country": "Belgium"
                }
              },
              "date": "2017-08-27",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "18",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "10",
                  "laps": "25",
                  "status": "Engine",
                  "FastestLap": {
                    "rank": "19",
                    "lap": "12",
                    "Time": {
                      "time": "1:51.720"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "225.692"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "13",
              "url": "https://en.wikipedia.org/wiki/2017_Italian_Grand_Prix",
              "raceName": "Italian Grand Prix",
              "Circuit": {
                "circuitId": "monza",
                "url": "http://en.wikipedia.org/wiki/Autodromo_Nazionale_Monza",
                "circuitName": "Autodromo Nazionale di Monza",
                "Location": {
                  "lat": "45.6156",
                  "long": "9.28111",
                  "locality": "Monza",
                  "country": "Italy"
                }
              },
              "date": "2017-09-03",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "17",
                  "positionText": "17",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "19",
                  "laps": "50",
                  "status": "Gearbox",
                  "FastestLap": {
                    "rank": "13",
                    "lap": "44",
                    "Time": {
                      "time": "1:25.871"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "242.861"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "14",
              "url": "https://en.wikipedia.org/wiki/2017_Singapore_Grand_Prix",
              "raceName": "Singapore Grand Prix",
              "Circuit": {
                "circuitId": "marina_bay",
                "url": "http://en.wikipedia.org/wiki/Marina_Bay_Street_Circuit",
                "circuitName": "Marina Bay Street Circuit",
                "Location": {
                  "lat": "1.2914",
                  "long": "103.864",
                  "locality": "Marina Bay",
                  "country": "Singapore"
                }
              },
              "date": "2017-09-17",
              "time": "12:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "17",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "8",
                  "laps": "8",
                  "status": "Collision damage",
                  "FastestLap": {
                    "rank": "17",
                    "lap": "6",
                    "Time": {
                      "time": "2:13.579"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "136.503"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "15",
              "url": "https://en.wikipedia.org/wiki/2017_Malaysian_Grand_Prix",
              "raceName": "Malaysian Grand Prix",
              "Circuit": {
                "circuitId": "sepang",
                "url": "http://en.wikipedia.org/wiki/Sepang_International_Circuit",
                "circuitName": "Sepang International Circuit",
                "Location": {
                  "lat": "2.76083",
                  "long": "101.738",
                  "locality": "Kuala Lumpur",
                  "country": "Malaysia"
                }
              },
              "date": "2017-10-01",
              "time": "07:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "11",
                  "positionText": "11",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "10",
                  "laps": "55",
                  "status": "+1 Lap",
                  "FastestLap": {
                    "rank": "11",
                    "lap": "11",
                    "Time": {
                      "time": "1:36.501"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "206.783"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "16",
              "url": "https://en.wikipedia.org/wiki/2017_Japanese_Grand_Prix",
              "raceName": "Japanese Grand Prix",
              "Circuit": {
                "circuitId": "suzuka",
                "url": "http://en.wikipedia.org/wiki/Suzuka_Circuit",
                "circuitName": "Suzuka Circuit",
                "Location": {
                  "lat": "34.8431",
                  "long": "136.541",
                  "locality": "Suzuka",
                  "country": "Japan"
                }
              },
              "date": "2017-10-08",
              "time": "05:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "11",
                  "positionText": "11",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "20",
                  "laps": "52",
                  "status": "+1 Lap",
                  "FastestLap": {
                    "rank": "12",
                    "lap": "45",
                    "Time": {
                      "time": "1:35.111"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "219.797"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "17",
              "url": "https://en.wikipedia.org/wiki/2017_United_States_Grand_Prix",
              "raceName": "United States Grand Prix",
              "Circuit": {
                "circuitId": "americas",
                "url": "http://en.wikipedia.org/wiki/Circuit_of_the_Americas",
                "circuitName": "Circuit of the Americas",
                "Location": {
                  "lat": "30.1328",
                  "long": "-97.6411",
                  "locality": "Austin",
                  "country": "USA"
                }
              },
              "date": "2017-10-22",
              "time": "19:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "17",
                  "positionText": "R",
                  "points": "0",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "8",
                  "laps": "24",
                  "status": "Engine",
                  "FastestLap": {
                    "rank": "18",
                    "lap": "21",
                    "Time": {
                      "time": "1:41.537"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "195.463"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "18",
              "url": "https://en.wikipedia.org/wiki/2017_Mexican_Grand_Prix",
              "raceName": "Mexican Grand Prix",
              "Circuit": {
                "circuitId": "rodriguez",
                "url": "http://en.wikipedia.org/wiki/Aut%C3%B3dromo_Hermanos_Rodr%C3%ADguez",
                "circuitName": "Aut\u00f3dromo Hermanos Rodr\u00edguez",
                "Location": {
                  "lat": "19.4042",
                  "long": "-99.0907",
                  "locality": "Mexico City",
                  "country": "Mexico"
                }
              },
              "date": "2017-10-29",
              "time": "19:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "10",
                  "positionText": "10",
                  "points": "1",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "18",
                  "laps": "70",
                  "status": "+1 Lap",
                  "FastestLap": {
                    "rank": "11",
                    "lap": "50",
                    "Time": {
                      "time": "1:21.014"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "191.255"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "19",
              "url": "https://en.wikipedia.org/wiki/2017_Brazilian_Grand_Prix",
              "raceName": "Brazilian Grand Prix",
              "Circuit": {
                "circuitId": "interlagos",
                "url": "http://en.wikipedia.org/wiki/Aut%C3%B3dromo_Jos%C3%A9_Carlos_Pace",
                "circuitName": "Aut\u00f3dromo Jos\u00e9 Carlos Pace",
                "Location": {
                  "lat": "-23.7036",
                  "long": "-46.6997",
                  "locality": "S\u00e3o Paulo",
                  "country": "Brazil"
                }
              },
              "date": "2017-11-12",
              "time": "16:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "8",
                  "positionText": "8",
                  "points": "4",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "6",
                  "laps": "71",
                  "status": "Finished",
                  "Time": {
                    "millis": "5555625",
                    "time": "+1:09.363"
                  },
                  "FastestLap": {
                    "rank": "10",
                    "lap": "57",
                    "Time": {
                      "time": "1:13.451"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "211.193"
                    }
                  }
                }
              ]
            },
            {
              "season": "2017",
              "round": "20",
              "url": "https://en.wikipedia.org/wiki/2017_Abu_Dhabi_Grand_Prix",
              "raceName": "Abu Dhabi Grand Prix",
              "Circuit": {
                "circuitId": "yas_marina",
                "url": "http://en.wikipedia.org/wiki/Yas_Marina_Circuit",
                "circuitName": "Yas Marina Circuit",
                "Location": {
                  "lat": "24.4672",
                  "long": "54.6031",
                  "locality": "Abu Dhabi",
                  "country": "UAE"
                }
              },
              "date": "2017-11-26",
              "time": "13:00:00Z",
              "Results": [
                {
                  "number": "14",
                  "position": "9",
                  "positionText": "9",
                  "points": "2",
                  "Driver": {
                    "driverId": "alonso",
                    "permanentNumber": "14",
                    "code": "ALO",
                    "url": "http://en.wikipedia.org/wiki/Fernando_Alonso",
                    "givenName": "Fernando",
                    "familyName": "Alonso",
                    "dateOfBirth": "1981-07-29",
                    "nationality": "Spanish"
                  },
                  "Constructor": {
                    "constructorId": "mclaren",
                    "url": "http://en.wikipedia.org/wiki/McLaren",
                    "name": "McLaren",
                    "nationality": "British"
                  },
                  "grid": "11",
                  "laps": "54",
                  "status": "+1 Lap",
                  "FastestLap": {
                    "rank": "4",
                    "lap": "26",
                    "Time": {
                      "time": "1:43.378"
                    },
                    "AverageSpeed": {
                      "units": "kph",
                      "speed": "193.410"
                    }
                  }
                }
              ]
            }
          ]
        }
      }
    }


This JSON response is quite long, containing a lot of information we're not going to use (interesting though). In Jupyter, this output would be contained in a scrollable cell. For this blog post I've shortened it manually in the mardown to just one result record. Let's strip out just the info we need from the full JSON response to a nice tidy dataframe.


```python
import pandas as pd
import matplotlib.pyplot as plt
```


```python
alonso_results_list = [
    {
        "round": int(race["round"]),
        "started": int(race["Results"][0]["grid"]),
        "finished": int(race["Results"][0]["position"])
    } for race in alonso_results["MRData"]["RaceTable"]["Races"]
]
alonso_results_list
```




    [{'finished': 14, 'round': 1, 'started': 12},
     {'finished': 16, 'round': 2, 'started': 13},
     {'finished': 14, 'round': 3, 'started': 15},
     {'finished': 20, 'round': 4, 'started': 15},
     {'finished': 12, 'round': 5, 'started': 7},
     {'finished': 16, 'round': 7, 'started': 12},
     {'finished': 9, 'round': 8, 'started': 19},
     {'finished': 19, 'round': 9, 'started': 12},
     {'finished': 18, 'round': 10, 'started': 20},
     {'finished': 6, 'round': 11, 'started': 7},
     {'finished': 18, 'round': 12, 'started': 10},
     {'finished': 17, 'round': 13, 'started': 19},
     {'finished': 17, 'round': 14, 'started': 8},
     {'finished': 11, 'round': 15, 'started': 10},
     {'finished': 11, 'round': 16, 'started': 20},
     {'finished': 17, 'round': 17, 'started': 8},
     {'finished': 10, 'round': 18, 'started': 18},
     {'finished': 8, 'round': 19, 'started': 6},
     {'finished': 9, 'round': 20, 'started': 11}]




```python
alonso_results_df = pd.DataFrame(alonso_results_list).set_index("round")
alonso_results_df
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
      <th>finished</th>
      <th>started</th>
    </tr>
    <tr>
      <th>round</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>14</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16</td>
      <td>13</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>16</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>19</td>
    </tr>
    <tr>
      <th>9</th>
      <td>19</td>
      <td>12</td>
    </tr>
    <tr>
      <th>10</th>
      <td>18</td>
      <td>20</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <th>12</th>
      <td>18</td>
      <td>10</td>
    </tr>
    <tr>
      <th>13</th>
      <td>17</td>
      <td>19</td>
    </tr>
    <tr>
      <th>14</th>
      <td>17</td>
      <td>8</td>
    </tr>
    <tr>
      <th>15</th>
      <td>11</td>
      <td>10</td>
    </tr>
    <tr>
      <th>16</th>
      <td>11</td>
      <td>20</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>8</td>
    </tr>
    <tr>
      <th>18</th>
      <td>10</td>
      <td>18</td>
    </tr>
    <tr>
      <th>19</th>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>20</th>
      <td>9</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



And then plot those results


```python
alonso_results_df.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f4b0d220e80>




![png](Blogging_from_Jupyter_files/Blogging_from_Jupyter_25_1.png)


All a bit up and down really.

Right that should be enough content for our blog.

## Step 2: Convert the Jupyter Notebook to a Markdown file

Jupyter comes with a command line tool called `nbconvert` to convert notebooks to a variety of other formats. Open a new terminal (you can do this from Jupyter beneath the options for new notebooks). Navigate to the location of your notebook and run the nbconvert utility as follows,

```bash
jupyter nbconvert --to=markdown Blogging_from_Jupyter.ipynb
```

You'll notice there is a now a file with the same name but a `.md` extension. There is also a folder called `Blogging_from_Jupyter_files`. This is where any images created by running the notebook (e.g. graphs) will be stored.

## Step 3: Open the Markdown file and edit file paths

## Step 4: Run Blogdown in RStudio to preview the results

## Step 5: Make a pull request to the StatsLab project
