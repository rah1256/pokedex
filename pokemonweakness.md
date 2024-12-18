`import requests

pokename = input()
pokename = pokename.lower()

dom = 'https://pokeapi.co/api/v2/pokemon/'
call = dom + pokename
print(call)

response = requests.get(call)
#print(response.status_code)
pokemon_data = response.json()
#print(pokemon_data)

name = pokemon_data['forms'][0]['name']
print (name)

types = pokemon_data['types']
type1 = types[0]['type']['name']
type2 = types[1]['type']['name'] if len(types) > 1 else None
print("Types:", type1, '&', type2 if type2 else "None")

types_to_weaknesses_map = {
    ('normal',): [('fighting', 2.0), ('ghost', 0)],
    ('fire',): [('fire', 0.5), ('water', 2.0), ('grass', 0.5), ('ice', 0.5), ('ground', 2.0), ('bug', 0.5), ('rock', 2.0), ('steel', 0.5), ('fairy', 0.5)],
    ('water',): [('fire', 0.5), ('water', 0.5), ('electric', 2.0), ('grass', 2.0), ('ice', 0.5), ('steel', 0.5)],
    ('electric',): [('electric', 0.5), ('ground', 2.0), ('flying', 0.5), ('steel', 0.5)],
    ('grass',): [('fire', 2.0), ('water', 0.5), ('electric', 0.5), ('grass', 0.5), ('ice', 2.0), ('poison', 2.0), ('ground', 0.5), ('flying', 2.0), ('bug', 2.0)],
    ('ice',): [('fire', 2.0), ('ice', 0.5), ('fighting', 2.0), ('rock', 2.0), ('steel', 2.0)],
    ('fighting',): [('flying', 2.0), ('psychic', 2.0), ('bug', 0.5), ('rock', 0.5), ('dark', 0.5), ('fairy', 2.0)],
    ('poison',): [('grass', 0.5), ('fighting', 0.5), ('poison', 0.5), ('ground', 2.0), ('psychic', 2.0), ('bug', 0.5), ('fairy', 0.5)],
    ('ground',): [('water', 2.0), ('electric', 0.0), ('grass', 2.0), ('ice', 2.0), ('poison', 0.5), ('rock', 0.5)],
    ('flying',): [('electric', 2.0), ('grass', 0.5), ('ice', 2.0), ('fighting', 0.5), ('ground', 0.0), ('bug', 0.5), ('rock', 2.0)],
    ('psychic',): [('fighting', 0.5), ('psychic', 0.5), ('bug', 2.0), ('ghost', 2.0), ('dark', 2.0)],
    ('bug',): [('fire', 2.0), ('grass', 0.5), ('fighting', 0.5), ('ground', 0.5), ('flying', 2.0), ('rock', 2.0)],
    ('rock',): [('normal', 0.5), ('fire', 0.5), ('water', 2.0), ('grass', 2.0), ('fighting', 2.0), ('poison', 0.5), ('ground', 2.0), ('flying', 0.5), ('steel', 2.0)],
    ('ghost',): [('normal', 0.0), ('fighting', 0.0), ('poison', 0.5), ('bug', 0.5), ('ghost', 2.0), ('dark', 2.0)],
    ('dragon',): [('fire', 0.5), ('water', 0.5), ('electric', 0.5), ('grass', 0.5), ('ice', 2.0), ('dragon', 2.0), ('fairy', 2.0)],
    ('dark',): [('fighting', 2.0), ('psychic', 0.0), ('bug', 2.0), ('ghost', 0.5), ('dark', 0.5), ('fairy', 2.0)],
    ('steel',): [('normal', 0.5), ('fire', 2.0), ('grass', 0.5), ('ice', 0.5), ('fighting', 2.0), ('poison', 0.0), ('ground', 2.0), ('flying', 0.5), ('psychic', 0.5), ('bug', 0.5), ('rock', 0.5), ('dragon', 0.5), ('steel', 0.5), ('fairy', 0.5)],
    ('fairy',): [('fighting', 0.5), ('poison', 2.0), ('bug', 0.5), ('dragon', 0.0), ('dark', 0.5), ('steel', 2.0)]
}

def get_weaknesses(types):
        weaknesses = {}
        for type_comb in types_to_weaknesses_map.keys():
            if all(t in types for t in type_comb):
                for weakness in types_to_weaknesses_map[type_comb]:
                    weaknesses[weakness[0]] = weaknesses.get(weakness[0], 1.0) * weakness[1]
        return weaknesses

weaknesses = get_weaknesses((type1, type2) if type2 else (type1,))
print("Weaknesses:", weaknesses)

`
