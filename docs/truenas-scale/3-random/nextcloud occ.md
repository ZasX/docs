# Run inside the container shell

## Current method

Use the hpb container
```
php occ db:add-missing-indices
```

## Old method
```
runuser -u www-data -- php occ db:add-missing-indices
```
