---
layout: post
title:  "Are Pokemon Obese?"
date: 13-11-2024
description: Analyzing heights, weights, and more across the different regions of the Pokemon world.
image: /assets/img/pokeball_cover.jpg
---

<p class="intro"><span class="dropcap">T</span>he world of Pokemon is vast, teeming with cute cartoon creatures of all shapes and sizes and from all sorts of places. When choosing Pokemon for your adventure, sometimes it can be difficult to know where to start. This post will serve as a guide to help you find which regions have the Pokemon that most fit your basic preferences!</p>

<img src="{{site.url}}/{{site.baseurl}}/assets/img/magikarp.png" alt="" style="width:500px;"/>

## The Burning Question

The reason I wanted to analyze Pokemon data is because I wanted to know: How do their heights, weights, and types differ depending on what region they are from?

### Ok, but how did I get this data?

All the data I have gathered comes from the Pokemon API site [PokeAPI](https://https://pokeapi.co/). This site is free to use and no API key is required! Still, in order to ensure my burden on the site was light, I limited my dataset to the first 5 regions of Pokemon, and only gathered data on a handful of different aspects about them (name, height, weight, type, and region).

I used Python's [requests](https://requests.readthedocs.io/en/latest/) library to retrieve the data I wanted. All of the code I used to do it can be found [here](https://github.com/evandrewer/pokemon-blogpost-code).

I first defined a function to request from the correct url:

```
def getPokemonData(endpoint):
    base_url = "https://pokeapi.co/api/v2/pokemon/"
    url = base_url + endpoint
    r = requests.get(url)
    return r.json()
```

This function returns a .json() file with the data we've requested ready to be extracted. I then created another code block to define the boundaries of each Pokemon's region by their IDs:

```
pokemon_ids = range(1, 650)

regions = {
    'Kanto': range(1, 152),
    'Johto': range(152, 252),
    'Hoenn': range(252, 387),
    'Sinnoh': range(387, 494),
    'Unova': range(494, 650)
}

def getRegion(pokemon_id):
    for region, id_range in regions.items():
        if pokemon_id in id_range:
            return region
    return None
```

After that, I put these functions into action! Using a for loop, I ran through each pokemon ID from the first 5 regions, extracting their name, height, weight, and type(s), and correctly attributing regions. After each loop, I appended all the gathered data to my list, `data`.

```
data = []
for pokemon_id in pokemon_ids:
    pokemon_data = getPokemonData(str(pokemon_id))

    if pokemon_data:
        name = pokemon_data['name'].capitalize()
        height = pokemon_data['height'] / 10 # convert to meters
        weight = pokemon_data['weight'] / 10 # convert to kilograms
        types = [t['type']['name'] for t in pokemon_data['types']]
        region = getRegion(pokemon_id)
        
        data.append({'pokemon': name, 'height': height, 'weight': weight, 'types': types, 'region': region})
```

Finally, I converted my list of lists into a proper DataFrame, set the index to correspond to each Pokemon's ID number, and exploded the DataFrame to account for Pokemon with 2 types.

```
df = pd.DataFrame(data)
df.index = pd.RangeIndex(start=1, stop=len(df)+1, step=1)
df_multitype = df.explode('types')
```

The final result is a Dataframe with 958 rows, containing the first 649 Pokemon!

### Some interesting findings

I made sure to use the unexploded dataframe for most of my analysis. Don't want any Pokemon having their non-type data accounted for twice.

#### Heights

I wanted to take a look at Pokemon's heights (meters) first. These are the mean, minimum, and maximum heights, grouped by region:

|        | Mean     | Minimum | Maximum |
| ------ | -------- | ------- | ------- |
| Kanto  | 1.194702 | 0.2     | 8.8     |
| Johto  | 1.163000 | 0.2     | 9.2     |
| Hoenn  | 1.229630 | 0.2     | 14.5    |
| Sinnoh | 1.133645 | 0.2     | 5.4     |
| Unova  | 1.032051 | 0.1     | 3.3     |

#### Weights

And here are the weights (kg):

|        | Mean      | Minimum | Maximum |
| ------ | --------- | ------- | ------- |
| Kanto  | 45.951656 | 0.1     | 460.0   |
| Johto  | 49.105000 | 0.5     | 400.0   |
| Hoenn  | 67.077778 | 0.8     | 950.0   |
| Sinnoh | 76.885047 | 0.3     | 750.0   |
| Unova  | 52.402564 | 0.3     | 345.0   |

#### Correlation

I was interested in how closely correlated weight and height are, so I graphed a quick scatterplot:

<img src="{{site.url}}/{{site.baseurl}}/assets/img/graph1.png" alt="" style="width:500px;"/>

The correlation coefficient is 0.63752. There appear to be a few outliers which are negatively affecting the correlation though. Here are the top 5 Pokemon for height and weight, respectively:

| pokemon  | height	| weight | types	        | region |
| -------- | ------ | ------ | ---------------- | ------ |
| Wailord  | 14.5	| 398.0	 | [water]	        | Hoenn  |
| Steelix  | 9.2	| 400.0	 | [steel, ground]  | Johto  |
| Onix	   | 8.8	| 210.0	 | [rock, ground]   | Kanto  |
| Rayquaza | 7.0	| 206.5	 | [dragon, flying] | Hoenn  |
| Gyarados | 6.5	| 235.0	 | [water, flying]	| Kanto  |


| pokemon          | height	| weight | types	        | region |
| ---------------- | ------ | ------ | ---------------- | ------ |
| Groudon          | 3.5	| 950.0	 | [ground]         | Hoenn  |
| Giratina-altered | 4.5	| 750.0	 | [ghost, dragon]  | Sinnoh |
| Dialga    	   | 5.4	| 683.0  | [steel, dragon]  | Sinnoh |
| Metagross        | 1.6	| 550.0	 | [steel, psychic] | Hoenn  |
| Snorlax          | 2.1	| 460.0	 | [normal]     	| Kanto  |

Due to the outliers potentially skewing the data, I removed them. Here's the new graph:

<img src="{{site.url}}/{{site.baseurl}}/assets/img/graph2.png" alt="" style="width:500px;"/>

The respective correlation coefficient for this one is 0.7244982, which indicates a strong positive correlation between Pokemon height and weight! Additionally, calculating the average BMI (kg/m^2) of Pokemon across these 5 regions gives us a whopping BMI of 43.62276! Seeing as the threshold for obesity is any BMI greater than 30, it looks like Pokemon may need to cut the junk food and start excercising regularly.

#### Types

Lastly, I wanted to know which types are most common for each region. Here are the top 3 most common types by region:

| region | type    | count |
| ------ | ------- | ----- |
| Kanto  | poison  | 33    |
|        | water   | 32    |
| 	     | normal  | 22    |
| Johto  | flying  | 19    |
|        | water   | 18    |
|        | normal  | 15    |
| Hoenn  | water   | 28    |
| 	     | psychic | 20    |
|        | normal  | 18    |
| Sinnoh | normal  | 17    |
|        | flying  | 14    |
|        | grass   | 14    |
| Unova  | grass   | 20    |
|        | bug     | 18    |
|        | flying  | 18    |

### To conclude

I found a lot of interesting things out about the first 5 regions from the Pokemon games. For example, I never knew that there were so many Poison-type pokemon in the first region, Kanto. That always seemed like a less common type, and it's not even in the top 3 of any other region! I also found it surprising that none of the top 5 Pokemon by heights were in the top 5 by weight. You'd think that with a strong positive correlation between the two, there'd be at least one in both camps.

I'd strongly recommend heading to the [PokeAPI](https://https://pokeapi.co/) site and try requesting some data for yourself! It's quite simple to use and very well documented, making it a very good API for beginners. If you're interested in any of the code I used for analysis, that can also be found in my github [here](https://github.com/evandrewer/pokemon-blogpost-code). Thanks for reading!