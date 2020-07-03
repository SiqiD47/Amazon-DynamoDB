## Cloud9 terminal: 

Partition key (required) + sort key (optional)

#### List table and add data

1. aws dynamodb list-tables

   ```
   {
     "TableNames": [
     "Music"
     ]
   }
   ```

   

2. aws dynamodb put-item --table-name Music --item file://song1.json

  ```json
  song1.json:
    {
      "Artist" : {"S" : "David Bowie"},
      "SongTitle" : {"S" : "Changes"},
      "AlbumTitle" : {"S" : "Hunky Dory"},
      "Genre" : {"S" : "Rock"}
    }
  ```

  

## DynamoDB console:

Click on the 'Items' tab and refresh the page to see the data.

## Could9 terminal

#### Query

1. aws dynamodb query --table-name Music --key-condition-expression "Artist = :v1" --expression-attribute-values file://values1.json

  ```json
  values1.json:
  	{
      ":v1" : {"S" : "David Bowie"}
  	}
  ```

  

2. aws dynamodb query --table-name Music --key-condition-expression "Artist = :v1 AND SongTitle = :v2" --expression-attribute-values file://values2.json

  ```json
  values2.json:
  	{
      ":v1" : {"S" : "David Bowie"},
      ":v2" : {"S" : "Heros"}
  	}
  ```

#### Delete

1. aws dynamodb delete-item --table-name Music --key file://keysToDelete.json

   ```json
   keysToDelete.json
   	{
       "Artist" : {"S" : "David Bowie"},
       "SongTitle" : {"S" : "Changes"}
   	}
   ```

#### Scan

*pull back all data*

1. aws dynamodb scan --table-name Music

#### Filter with scan

*go through every item of the table*

1. aws dynamodb query --table-name Music --key-condition-expression "Artist = :v1" --filter-expression "Released = :v2" --expression-attribute-values file://filterValues1.json

   ```json
   filterValues1.json
   	{
       ":v1" : {"S" : "Bryan Adams"},
       ":v2" : {"S" : "1998"}
   	}
   ```

2. aws dynamodb scan --table-name Music --filter-expression "SongTitle = :v1" --expression-attribute-values file://filterValues2.json

   ```
   filterValues2.json
   	{
       ":v1" : {"S" : "Heroes"}
   	}
   ```

#### Secondary index

1. Local secondary index: allows to pick alternate sort key

2. Global secondary index: allows to create alternate partition and sort key
3. **Can't query non-key attribute without a scan**

