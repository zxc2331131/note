# Day 2: Data Types

**TASK:**
Given the meal price (base cost of a meal), tip percent (the percentage of the meal price being added as tip), and tax percent (the percentage of the meal price being added as tax) for a meal, find and print the meal's total cost. Round the result to the nearest integer.

PHP:
```
<?php

/*
 * Complete the 'solve' function below.
 *
 * The function accepts following parameters:
 *  1. DOUBLE meal_cost
 *  2. INTEGER tip_percent
 *  3. INTEGER tax_percent
 */

function solve($meal_cost, $tip_percent, $tax_percent) {
    // Write your code here
    $total_cost = 0;
    $total_cost = $tip_percent*0.01*$meal_cost + $meal_cost + $tax_percent*0.01*$meal_cost;
    $total_cost = round($total_cost);
    return $total_cost;
    

}

$meal_cost = doubleval(trim(fgets(STDIN)));

$tip_percent = intval(trim(fgets(STDIN)));

$tax_percent = intval(trim(fgets(STDIN)));

$total = solve($meal_cost, $tip_percent, $tax_percent);
print($total);
```

C:
```

```
Python:
```

```