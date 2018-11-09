# a way to graphql query on empty array yaml data

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
