---
layout: post
title:  "The Pokemon Obesity Epidemic"
date: 13-11-2024
description: Analyzing heights, weights, and more across the different regions of the Pokemon world.
image: /assets/img/pokeball_cover.jpg
---

<p class="intro"><span class="dropcap">T</span>he world of Pokemon is vast, teeming with cute cartoon creatures of all shapes and sizes and from all sorts of places. When choosing Pokemon for your adventure, sometimes it can be difficult to know where to start. This post will serve as a guide to help you find which regions have the Pokemon that most fit your basic preferences!</p>

<img src="{{site.url}}/{{site.baseurl}}/assets/img/magikarp.png" alt="" style="width:700px;"/>

### The Burning Question

The reason I wanted to analyze Pokemon data is because I wanted to know: How do their heights, weights, and types differ depending on what region they are from?

### Ok, but how did you get all this data?

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

##### Heights

I wanted to take a look at Pokemon's heights first. These are the mean, minimum, and maximum heights, grouped by region:

|        | Mean     | Minimum | Maximum |
| ------ | -------- | ------- | ------- |
| Kanto  | 1.194702 | 0.2     | 8.8     |
| Johto  | 1.163000 | 0.2     | 9.2     |
| Hoenn  | 1.229630 | 0.2     | 14.5    |
| Sinnoh | 1.133645 | 0.2     | 5.4     |
| Unova  | 1.032051 | 0.1     | 3.3     |

