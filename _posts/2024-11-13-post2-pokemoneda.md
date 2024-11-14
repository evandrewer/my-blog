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

### How did you get all this data?

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

This function returns the .json() file with the data ready to be extracted. I then created another code block to define the boundaries of each Pokemon's region by their IDs:

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