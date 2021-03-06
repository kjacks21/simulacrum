###Simulacrum
Simulacrum is a simple way to pass in a dictionary object, with column names and corresponding data types and output a pandas
DataFrame of random data. This is great for creating a fake data set or testing a data science script whose validity through
generalization needs to be tested. This is still a work in Progress. right now numerical data can be made to fit a common
statistical distribution so long as the proper statistical parameters are included with the type key in the dictionary.

simulacrum fulfills the following use cases:
- When data is needed for a tutorial, in order to train a model perfectly
- When real data that is well understood cannot be gathered fast enough, but can be simulated quickly
- to develop test case datasets for testing machine learning pipelines and applications on more generalizable data


For Instance, companies that operate primarily as brick and mortars cannot gather data at
the same rate as ecommerce companies typically. Gathering customer data may take months. Therefore the data to develop machine learning applications may not exist at
present but the company may have a general understanding of the shape of this data, and can thus simulate it. A model can be
trained on this simulated data and a machine learning application can be developed in parallel to gathering real data! This
hopefully engineers scalability and foresight for companies with slow data velocity.

###Installation

```
$ pip install simulacrum
```

###Usage
To create a fake pandas dataframe consisting of 100 rows. we would do the following:
```python

import simulacrum as sm

test = {'entries': {'type': 'exp', 'lam': 0.5},
        'names': {'type': 'name'},
        'salaries': {'type': 'norm', 'mean': 55000, 'sd': 20000}}

res = sm.create(100, coltypes=test)
```
For the test variable which will be passed as coltypes, each key corresponds to the column name. Each value must be a dictionary
with one key being type and then whatever statistical parameters if any correspond to that type. Please review source code to
see how to properly pass the correct keys and values. Possible types are as follows:

`{'num': num_data, 'int': num_int, 'norm': norm_data, 'exp': exp_data, 'bin': binom_data, 'pois': poisson_data, 'txt': text_data, 'name': name_data, 'addr': address_data, 'zip': zip_data, 'uuid': uuid_data, 'faker': faker_data}`

Each key corresponds to a possible type, and the value is the function called in the source.

We can also use the ColTypes class to create the coltypes dict with function calls.

```python

import simulacrum as sm

col_types = sm.ColTypes()
col_types.add_coltype('ids', 'uuid')
col_types.add_coltype('name', 'name')
col_types.add_coltype('salaries', 'norm', mean=50000, sd=1000)
col_types.add_coltype('ips', 'faker', provider='ipv6')

data_set = sm.create(1000, coltypes=col_types.get_coltypes())
```

#### faker type:

If you want to use other data types not allowed in simulacrum by default, you can use the `faker` type to use each data type provided by the awesome faker library.

**List of faker data types : https://faker.readthedocs.io/en/latest/providers.html**

To use the faker type, you have to pass the provider name and optional args like that:

```python
faker_examples = {
        # same as fake.simple_profile(sex='F')
        'names': {'type': 'name', 'provider': 'simple_profile', 'sex': 'F'},
        # same as fake.ipv6()
        'ips': {'type': 'faker', 'provider': 'ipv6'},
        # same as fake.job()
        'jobs': {'type': 'faker', 'provider': 'job'},
        # same as fake.pydict(nb_elements=10, variable_nb_elements=True)
        'metadata': {'type': 'faker', 'provider': 'pydict', 'nb_elements': 10, 'variable_nb_elements': True}
}

# Or with ColTypes
col_types.add_coltype('ids', 'faker', provider='pydict', nb_elements=10, variable_nb_elements=True)
```

###TODO
- Add a error handling and validation for date_data function
- Add a function for fake coordinates
- Add a function for fake zipcodes
- Add a function for fake categorical variables
- Add Try Except blocks to all functions to log errors when params passed in dict dont match expected params


###Development

```
$ git clone https://github.com/jbrambleDC/simulacrum.git && cd simulacrum
$ python setup.py install
```
if you want to contribute. Checkout a branch off of master, and open a PR!
