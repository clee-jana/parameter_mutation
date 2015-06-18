## Alternatives to
## Parameter Mutation

---

"Practical functional programming is
__not about eliminating state change__, but instead about
__reducing the occurrences of mutation to the smallest area__
possible."


_Michael Fogus, "Functional Javascript"_

---

```
def get_cpe_list_for_display(cpe_offers):
    cpe_list = []
    for offer in cpe_offers:
        add_cpe_list_item(offer, cpe_list)

    return cpe_list


# function name tells you something is mutating
def add_cpe_list_item(cpe_data, cpe_list):
    list_item = {}
    list_item['offer_id'] = cpe_data['offer_id']
    list_item['directive'] = get_directive(cpe_data)

    cpe_list.append(list_item)
```

_this would work_
---

## reduce __unexpected__ mutation

---

```
def get_cpe_list_for_display(cpe_offers):
    cpe_list = []
    for offer in cpe_offers:
        get_cpe_list_item(offer, cpe_list)

    return cpe_list


# bad function name leads to unexpected behavior
def get_cpe_list_item(cpe_data, cpe_list):
    list_item = {}
    list_item['offer_id'] = cpe_data['offer_id']
    list_item['directive'] = get_directive(cpe_data)

    # unexpected mutation here
    cpe_list.append(list_item)
```

---

```
def get_cpe_list_for_display(cpe_offers):
    cpe_list = []
    for offer in cpe_offers:
        # this is a little bit clearer that cpe_list is changing
        cpe_list = get_cpe_list_item(offer, cpe_list)

    return cpe_list


def get_cpe_list_item(cpe_data, cpe_list):
    list_item = {}
    list_item['offer_id'] = cpe_data['offer_id']
    list_item['directive'] = get_directive(cpe_data)

    cpe_list.append(list_item)
    return cpe_list
```

---

## Pretend the parameters are immutable

---

```
def get_cpe_list_for_display(self, cpe_offers):
    cpe_list = []
    for offer in cpe_offers:
        # moved control of the append to the caller
        cpe_list.append(get_cpe_list_item(offer))

    return cpe_list

# removed an argument to this function
def get_cpe_list_item(cpe_data):
    list_item = {}
    list_item['offer_id'] = cpe_data['offer_id']
    list_item['directive'] = get_directive(cpe_data)

    return list_item
```

---
## Why?

## Composability improves Flexibility

---

```
def get_cpe_dict_for_display(self, cpe_offers):
    # what if we wanted to make this a dict?
    cpe_offers = {}
    for offer in cpe_offers:
        cpe_offers[offer.id] = get_cpe_list_item(offer)

    return cpe_offers

# no changes required here
def get_cpe_list_item(cpe_data):
    list_item = {}
    list_item['offer_id'] = cpe_data['offer_id']
    list_item['directive'] = get_directive(cpe_data)

    return list_item
```

---

* Easier to read and understand
* Easier to write tests
* Easier to name (single responsibility principle)
* Better decoupling and improved flexibility

:thumbsup:


