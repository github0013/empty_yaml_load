# A way to query on an empty array yaml data

Yaml files are in [src/data][1]. If you simply create a yaml file with an empty array, the yaml file won't be picked up as a graphql query type at load.

e.g.

```
# src/data/empty_array.yml

[]
```

query like this will fail since `allEmptyArrayYaml` itself doesn't exist.

```
{
  allEmptyArrayYaml{
    edges{
      node{
        character
      }
    }
  }
}
```

To avoid this query type mis-load, I placed a dummy type definition yaml (`type_def.yml`) file in [src/data/array][2] and [src/data/empty_array][3]. Then, when to query, filter the definition data to fetch only needed.

e.g.

```
{
  # unfilterd all results
  allArrayYaml{
    edges{
      node{
        data{
          character
        }
      }
    }
  }

  # unfilterd all results
  allEmptyArrayYaml{
    edges{
      node{
        data{
          character
        }
      }
    }
  }

  # remove definition data
  # shows array data in object.yml
  filteredAllArrayYaml: allArrayYaml(filter:{ data:{elemMatch:{typeDef: {ne: true}}}}){
    edges{
      node{
        data{
          character
        }
      }
    }
  }

  # remove definition data
  # returns null instead of [], but it won't cause a build error
  filteredAllEmptyArrayYaml: allEmptyArrayYaml(filter:{ data:{elemMatch:{typeDef: {ne: true}}}}){
    edges{
      node{
        data{
          character
        }
      }
    }
  }
}
```

[1]: https://github.com/github0013/empty_yaml_load/tree/master/src/data
[2]: https://github.com/github0013/empty_yaml_load/tree/master/src/data/array
[3]: https://github.com/github0013/empty_yaml_load/tree/master/src/data/emtpy_array
