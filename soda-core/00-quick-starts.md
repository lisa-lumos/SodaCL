# 0. Quick starts
## Intro
Soda Checks Language (SodaCL) is a YAML-based language for data reliability. You use SodaCL to write checks for data quality, then run a scan of the data in your data source to execute those checks.

A Soda Check is a test that Soda performs when it scans a dataset in your data source. A Soda scan executes the checks you defined, and returns a result for each check: pass, fail, or error. Optionally, you can configure a check to warn instead of fail, by setting an alert config.

soda core is an open-source, CLI tool and Python library for data reliability. 

Example checks:
```yml
# Checks for basic validations
checks for dim_customer:
  - row_count between 10 and 1000
  - missing_count(birth_date) = 0
  - invalid_percent(phone) < 1 %:
      valid format: phone number
  - invalid_count(number_cars_owned) = 0:
      valid min: 1
      valid max: 6
  - duplicate_count(phone) = 0
checks for dim_product:
  - avg(safety_stock_level) > 50
```

```yml
# Check for schema changes
checks for dim_product:
  - schema:
      name: Find forbidden, missing, or wrong type
      warn:
        when required column missing: [dealer_price, list_price]
        when forbidden column present: [credit_card]
        when wrong column type:
          standard_cost: money
      fail:
        when forbidden column present: [pii*]
        when wrong column index:
          model_name: 22
```

```yml
# Check for freshness 
checks for dim_product:
  - freshness(start_date) < 1d
```

```yml
# Check for referential integrity
checks for dim_department_group:
  - values in (department_group_name) must exist in dim_employee (department_name)
```



























