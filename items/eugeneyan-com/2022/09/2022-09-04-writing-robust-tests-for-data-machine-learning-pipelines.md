---
title: Writing Robust Tests for Data & Machine Learning Pipelines
link: https://eugeneyan.com//writing/testing-pipelines/
source: eugeneyan-com
published: 2022-09-04T00:00:00Z
updated: 2022-09-04T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- machinelearning
- production
- 🩷
summary: Or why I should write fewer integration tests.
content: extracted
html: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.html
preview:
  file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.preview-09e0b76af23c.webp
  width: 256
  height: 134
  color: '#5d5d64'
images:
- source: https://eugeneyan.com/assets/og_image/testing-pipelines.jpg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-13e664c4773e.jpg
    width: 1200
    height: 630
  color: '#050506'
- source: https://eugeneyan.com/assets/netflix-recommendation-slate.webp
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-24d03ae8c1b0.webp
    width: 1200
    height: 173
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-18b9d40a75e6.webp
    width: 320
    height: 46
  color: '#141515'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2022-09-04-writing-robust-tests-for-data-machine-learning-pipelines.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Recently, I’ve been pondering on how data and machine learning pipelines are tested, especially why some tests break more often than others. And it’s not because the new code is wrong; often, the new code is correct but the tests break anyway and need to be updated.

Here, I dig into why certain tests break—incorrectly—more than others and try to find a better way to test pipelines. We’ll start with a simple pipeline and test it via unit, schema, and integration tests. Then, I’ll introduce new data and logic, observe how tests break, and draw patterns from it. Finally, I’ll suggest how to make pipeline testing less brittle.

- [Overview of testing scopes: unit, integration, functional, etc.](https://eugeneyan.com//writing/testing-pipelines/#smaller-testing-scopes--shorter-feedback-loops)
- [An example pipeline: behavioral logs -> batch inference output](https://eugeneyan.com//writing/testing-pipelines/#example-pipeline-behavioral-logs---inference-output)
- [Writing tests for our pipeline: unit, schema, integration](https://eugeneyan.com//writing/testing-pipelines/#implementation-tests-unit-schema-integration)
- [Adding new data (visible impressions) and logic to our pipeline](https://eugeneyan.com//writing/testing-pipelines/#adding-new-data-or-logic--updating-pipeline-code)
- [The additive vs. retroactive impact of new data and logic on tests](https://eugeneyan.com//writing/testing-pipelines/#adding-new-datalogic-additive-vs-retroactive-impacts)
- [Related concepts: Test validity, granularity, and scope](https://eugeneyan.com//writing/testing-pipelines/#related-test-validity-granularity-and-scope)
- [Suggestions for more robust pipeline tests](https://eugeneyan.com//writing/testing-pipelines/#many-row-level-and-schema-tests-a-handful-of-the-rest)

## Smaller testing scopes = Shorter feedback loops

Before we jump into it, here’s a brief overview of various testing scopes (an Atlassian [article](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing) lists as many as seven of them). If we’re building a recommendation system pipeline, here’s how the various scopes map to concrete tests:

- Unit: Test individual methods and classes
- Integration: Test pipeline integration points, such as between data processing and model training pipelines, or checking that trained models are deployed correctly
- Functional: Test that business requirements are met (e.g., excluding erotica, rolling up movies from the same series, updating recs based on in-session clicks, etc.)
- End-to-end: Test that the pipeline output/endpoint integrates correctly with the site and recommendations are served correctly based on user ID or behavior
- Acceptance: Visual inspection and quality assurance of live recommendation slate
- Performance: Load testing and assessing latency and throughput
- Smoke: Handcrafted URLs to trigger recommendations on commonly QA-ed items
- A/B: Evaluate the impact of pipeline or model changes

Tests increase in scope as we go down the list. Bigger scopes lead to longer feedback loops: A unit test runs in milliseconds while an A/B test requires time to implement the change plus whatever period the A/B test needs to run for.

**Here, we’ll focus on tests with the shortest feedback loops: unit tests and integration tests.** Specifically, we’ll discuss tests that can be run offline, either on our local machine or a lightweight cloud instance. (Although integration tests can also run in a CI environment that calls other services or queries a database, we’ll exclude those.)

Also, though data and machine learning pipelines often have additional tests on data quality (e.g., [Great Expectations](https://greatexpectations.io), [Deequ](https://github.com/awslabs/deequ)), model evaluation (e.g., [scikit-learn metrics](https://scikit-learn.org/stable/modules/model_evaluation.html)), and [model behavioral checks](https://eugeneyan.com/writing/testing-ml/#post-train-tests-to-ensure-expected-learned-behaviour) (e.g., [CheckList](https://github.com/marcotcr/checklist), [RecList](https://reclist.io)), these tend to run in production with each new batch of data or model refresh. Thus, we’ll not discuss them here.

## Example pipeline: Behavioral logs -> inference output

At a high level, our example pipeline (i) consumes request-level behavioral logs, (ii) transforms them into item-level events, (iii) trains a naive model that estimates click-through rate (CTR), and (iv) performs batch inference on a list of items.

Here’s what the **behavioral logs** look like; this is also the sample data we’ll use in our tests. They track each time a recommendation slate is requested and the items that have been impressed or clicked. (The raw form of such logs is usually unstructured but in this pipeline they’ve been parsed and stored in a nice tabular format.)

```python
+--------------+--------------------------+--------------+--------------+
| request_id   | impressions              | event_item   | event_type   |
|--------------+--------------------------+--------------+--------------|
| r1           | ['i1', 'i2', 'i3', 'i4'] |              | impress      |
| r2           | ['i2', 'i3', 'i4', 'i1'] | i2           | click        |
| r3           | ['i3', 'i4', 'i1', 'i2'] |              | impress      |
| r4           | ['i4', 'i1', 'i2', 'i3'] | i3           | click        |
| r5           | ['i1', 'i2', 'i3', 'i4'] |              | impress      |
+--------------+--------------------------+--------------+--------------+
```

A brief description of each column:

- `request_id`: ID of recommendation slate request
- `impressions`: IDs of impressed items
- `event_item`: ID of clicked items; empty otherwise
- `event_type`: Type of event (i.e., impress or click)

For request `r1`, four items (`i1`, `i2`, `i3`, `i4`) were impressed but none were clicked. In `r2`, four items (`i2`, `i3`, `i4`, `i1`) were impressed and `i2`—the item in the first position—was clicked.

To make it easier to estimate item CTR, we want to transform the *request-level* behavioral logs into *item-level* events where each impression and click has its own row. In addition, we also want to get the position of the item that was impressed or clicked.

Here’s what the **events** table looks like for `r2` after the transformation:

```python
+--------------+--------+------------+--------------+
| request_id   | item   |   position | event_type   |
|--------------+--------+------------+--------------|
| r2           | i2     |          1 | impress      |
| r2           | i3     |          2 | impress      |
| r2           | i4     |          3 | impress      |
| r2           | i1     |          4 | impress      |
| r2           | i2     |          1 | click        |
+--------------+--------+------------+--------------+
```

Next, we train a naive model that learns historical CTR and estimates item-level CTR. This model is then used to perform batch inference on a list of item IDs.

Here’s what the **batch inference output** looks like. Notice that the expected CTR for `i5` is -1. This is because we only have behavioral logs for `i1` to `i4`—thus, `i5` is a new or cold-start item. Since our model can’t estimate CTR for `i5`, it returns a predefined null value (-1).

```python
+-----------+----------------+
| item_id   |   expected_ctr |
|-----------+----------------|
| i1        |            0   |
| i2        |            0.2 |
| i3        |            0.2 |
| i4        |            0   |
| i5        |           -1   |
+-----------+----------------+
```

So far, I’ve focused on sharing sample data at various stages in our example pipeline. If you prefer to look at code to help you understand better, please refer to `pipeline.py` [here](https://github.com/eugeneyan/testing-pipelines/blob/main/src/pipeline.py). Note that the code is deliberately kept simple as its implementation isn’t key in this write-up.

## Implementation tests: Unit, Schema, Integration

To test our pipeline, we’ll add (i) row-level unit tests, (ii) column-level unit tests, (iii) table-level unit tests, (iv) schema tests, and (v) integration tests. We use basic Pandas ([assert\_frame\_equal](https://pandas.pydata.org/docs/reference/api/pandas.testing.assert_frame_equal.html)) and PyTest ([fixtures](https://docs.pytest.org/en/6.2.x/fixture.html)) functionality. See test fixtures on [GitHub](https://github.com/eugeneyan/testing-pipelines/blob/main/tests/test_pipeline.py#L9).

Let’s start with the smallest form, **row-level unit tests**, where we test a method that takes an input (row) and returns a single value. We’ll test `get_click_position` which takes an array of impressions and the clicked item, and returns the position of the clicked item. Writing the test for this is straightforward and we can parametrize our test with various inputs and expected outputs.

```python
def get_click_position(impressions: List[str], click: str) -> int:
    """Returns the position of the clicked item based on the array of impressions."""
    try:
        return impressions.index(click) + 1
    except ValueError:
        return -1

    
# Unit test: Row level
@pytest.mark.parametrize('impressions,click,expected',
                         [(['i1', 'i2', 'i3', 'i4'], 'i1', 1),
                          (['i1', 'i2', 'i3', 'i4'], 'i3', 3),
                          (['i1', 'i2', 'i3', 'i4'], None, -1),
                          (['i1', 'i2', 'i3', 'i4'], 'NA', -1)])
def test_get_click_position(impressions, click, expected):
    assert get_click_position(impressions, click) == expected
```

Next, we add **column-level unit tests** for methods that take a column or table and return a column. The `get_impress_positions` method takes a table of request IDs and impression events and returns a column of each impression event’s position. Because the expected output is a column instead of a single value, the test starts to get unwieldy and it can be tedious to define expected outputs.

```python
def get_impress_positions(df: pd.DataFrame) -> pd.Series:
    """Returns a column of impression positions.

       Note: This method assumes that impressions are sorted in ascending order of their position.
    """
    positions = df.groupby('request_id').cumcount() + 1
    positions = positions.reset_index(drop=True)

    return positions


# Unit test: Column level
def test_get_impress_position(logs):
    impress_events = logs.explode('impressions')
    impress_positions = get_impress_positions(impress_events)

    # Since each impress log has 4 items, expect impression positions to be five sets of 1-4
    pd.testing.assert_series_equal(impress_positions, pd.Series([1, 2, 3, 4] * 5))
```

Next, we add **table-level unit tests** for methods that take a table, do some aggregation or filtering, and return another table. The `aggregate_events` method takes a table of impression and click events and aggregates them at the item level like below.

```python
+--------+---------+-----------+
| item   |   click |   impress |
|--------+---------+-----------|
| i1     |       0 |         5 |
| i2     |       1 |         5 |
| i3     |       1 |         5 |
| i4     |       0 |         5 |
+--------+---------+-----------+
```

Here’s the method and its unit test. Table-level unit tests are even more clunky as we need to define entire tables as expected output.

```python
def aggregate_events(events: pd.DataFrame) -> pd.DataFrame:
    """Returns a table of items and their aggregated impressions and clicks."""
    events_agg = events.pivot_table(index=['item'], columns=['event_type'], values=['request_id'],
                                    aggfunc='count', fill_value=0)
    events_agg.columns = events_agg.columns.droplevel()
    events_agg = events_agg.reset_index()

    # Clear index name
    events_agg = events_agg.rename_axis(None, axis=1)

    return events_agg


# Unit test: Table level
def test_aggregate_events(events):
    result = aggregate_events(events)

    arr = [['i1', 0, 5],
           ['i2', 1, 5],
           ['i3', 1, 5],
           ['i4', 0, 5]]
    cols = ['item', 'click', 'impress']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)
```

We also add **schema tests** at key pipeline junctures. Here, we check for expected column names and datatypes after transforming the request-level behavioral logs to item-level events. This test checks for a *minimum* set of columns: the pipeline can be updated without failing this test so long as the minimum set of columns is present with the right datatypes.

```python
# Schema check: Columns and datatypes
def test_events_schema(events):
    expected_cols = {'request_id': np.object,
                     'item': np.object,
                     'position': np.int64,
                     'event_type': np.object}

    # Check all expected columns are present
    assert len(set(expected_cols.keys()) - set(events.columns)) == 0, \
        f'{set(expected_cols.keys()) - set(events.columns)} columns missing!'

    # Check all column data types are correct
    for col, dtype in expected_cols.items():
        assert events.dtypes[col] == dtype, \
            f'Expected column {col} to be of {dtype} type but found {events.dtypes[col]} type!'
```

Finally, we add **integration tests** that take a fixed sample input, run it through the pipeline, and compare the result with the expected output. Below, we test that behavioral logs are transformed into events correctly.

```python
# Integration test: Input logs to aggregated events
def test_feature_pipeline(logs):
    impress_logs = logs
    click_logs = logs[logs['event_type'] == 'click']

    impress_events = get_impress_item_and_pos(impress_logs)
    click_events = get_click_item_and_pos(click_logs)

    sample_events = pd.concat([impress_events, click_events])
    result = aggregate_events(sample_events)

    arr = [['i1', 0, 5],
           ['i2', 1, 5],
           ['i3', 1, 5],
           ['i4', 0, 5]]
    cols = ['item', 'click', 'impress']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)
```

We also test that our model learns and returns the expected CTR estimates correctly.

```python
# Integration test: Aggregated events to batch inference
def test_model_pipeline(events, model, items):
    model = model.fit(events)
    result = model.batch_predict(items)

    arr = [['i1', 0.0],
           ['i2', 0.2],
           ['i3', 0.2],
           ['i4', 0.0],
           ['i5', -1.0]]
    cols = ['item_id', 'expected_ctr']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)
```

Integration tests are likely the most onerous of the lot as they rely on multiple data transforms and training steps working correctly. Thus, they can become unmanageable if the pipeline is updated frequently and they break just as often. Nonetheless, I find them helpful in ensuring the pipeline works as intended. Their expected output also helps explain how the pipeline should work.

Here’s the result of running our [tests](https://github.com/eugeneyan/testing-pipelines/blob/main/tests/test_pipeline.py).

```bash
> pytest -v tests/test_pipeline.py --cov=src --cov-report=term-missing

Name              Stmts   Miss  Cover   Missing
-----------------------------------------------
src/__init__.py       0      0   100%
src/pipeline.py      42      0   100%
-----------------------------------------------
TOTAL                42      0   100%
```

Aside: In this simple example, the pipeline code fits in a single script. In an industrial setting, the pipeline may span multiple repos and infra such as Spark (Scala, SQL) and SageMaker (Python, Docker). It’s possible to set up and run offline tests across multiple repos, but depending on your application and scale, it may not be worth the effort.

## Adding new data or logic = Updating pipeline code

In the current pipeline, the logged impressions are *server-side* impressions: All items in the recommendation slate are logged as impressed on customers *even if the customer has not actually seen the item*.

For example, on Netflix, only the first six items in a recommendation slate are visible on the screen; I need to side scroll to view the rest of the items. With server-side impressions, even if I didn’t side scroll, all items in the recommendation slate would be logged as impressed. This leads to a discrepancy between what we think the customer saw vs. what they really saw, especially on smaller screens such as mobile phones.

![Image](https://eugeneyan.com/assets/netflix-recommendation-slate.webp "Image")

Only a handful of items in a recommendation slate are visible if users don't side scroll.

To address this, we add additional data to our pipeline: *client-side* impressions. This data tracks which items were actually visible on the screen. Thus, if I didn’t side scroll on the recommendation slate, only the first six items would be logged as client-side impressions.

Here’s what the **updated behavioral logs** look like. We have a new column for visible impressions. (But client-side data’s not perfect which explains why `r3` has a null value.)

```python
+--------------+--------------------------+--------------------------+--------------+--------------+
| request_id   | impressions              | impressions_visible      | event_item   | event_type   |
|--------------+--------------------------+--------------------------+--------------+--------------|
| r1           | ['i1', 'i2', 'i3', 'i4'] | ['i1', 'i2', 'i3']       |              | impress      |
| r2           | ['i2', 'i3', 'i4', 'i1'] | ['i2', 'i3']             | i2           | click        |
| r3           | ['i3', 'i4', 'i1', 'i2'] |                          |              | impress      |
| r4           | ['i4', 'i1', 'i2', 'i3'] | ['i4', 'i1', 'i2', 'i3'] | i3           | click        |
| r5           | ['i1', 'i2', 'i3', 'i4'] | ['i1', 'i2']             |              | impress      |
+--------------+--------------------------+--------------------------+--------------+--------------+
```

We’ll want to update our pipeline to only consider impressions that were visible via some basic logic. Thus, we add methods to update impressions (at the row level) before updating the impressions column (at the table level).

```python
def get_updated_impressions(impressions: List[str], impressions_visible: List[str]) -> List[str]:
    """Returns an array of updated impressions based on logs of visible impressions."""
    if isinstance(impressions_visible, list) and len(impressions) >= len(impressions_visible):
        return impressions_visible
    return impressions


def update_impression_col(df: pd.DataFrame) -> pd.DataFrame:
    """Returns a table of logs where impressions is updated to consider visible impressions."""
    _df = df.copy()
    _df['impressions'] = _df.apply(lambda x: get_updated_impressions(x['impressions'],
                                                                     x['impressions_visible']), axis=1)
    _df = _df.drop(columns=['impressions_visible'])
    return _df
```

When we try to run our existing tests again with our new [input](https://github.com/eugeneyan/testing-pipelines/blob/main/tests/test_pipeline_v2.py#L9) and [pipeline code](https://github.com/eugeneyan/testing-pipelines/blob/main/src/pipeline_v2.py), not counting the parameterization, **4/6 tests fail because of the new data and logic**.

```plaintext
> pytest -v tests/test_pipeline.py --cov=src --cov-report=term-missing

tests/test_pipeline.py::test_get_click_position[impressions0-i1-1] PASSED       [ 11%]
tests/test_pipeline.py::test_get_click_position[impressions1-i3-3] PASSED       [ 22%]
tests/test_pipeline.py::test_get_click_position[impressions2-None--1] PASSED    [ 33%]
tests/test_pipeline.py::test_get_click_position[impressions3-NA--1] PASSED      [ 44%]
tests/test_pipeline.py::test_get_impress_position FAILED                        [ 55%]
tests/test_pipeline.py::test_aggregate_events FAILED                            [ 66%]
tests/test_pipeline.py::test_events_schema PASSED                               [ 77%]
tests/test_pipeline.py::test_feature_pipeline FAILED                            [ 88%]
tests/test_pipeline.py::test_model_pipeline FAILED                              [100%]
```

## Adding new data/logic: Additive vs. retroactive impacts

Since we’ve updated our pipeline, we’ll need to **add tests** for the `get_updated_impressions` method which is easy to parameterize.

```python
def get_updated_impressions(impressions: List[str], impressions_visible: List[str]) -> List[str]:
    """Returns an array of updated impressions based on logs of visible impressions."""
    if isinstance(impressions_visible, list) and len(impressions) >= len(impressions_visible):
        return impressions_visible
    return impressions


# Unit test: Row level (added)
@pytest.mark.parametrize('impressions,impressions_visible,updated_impressions',
                         [(['i1', 'i2', 'i3', 'i4'], ['i1', 'i2', 'i3'], ['i1', 'i2', 'i3']),
                          (['i1', 'i2'], ['i1', 'i2', 'i3'], ['i1', 'i2']),
                          (['i1', 'i2', 'i3', 'i4'], None, ['i1', 'i2', 'i3', 'i4'])])
def test_get_updated_impressions(impressions, impressions_visible, updated_impressions):
    assert get_updated_impressions(impressions, impressions_visible) == updated_impressions
```

What about the existing tests?

**Existing row-level unit tests are unchanged.** Because the tested methods only depend on row-level input, the test is invariant to new data. Barring incorrect data, the logic to compute click position isn’t coupled to whether the impression is visible or not.

**Existing column-level unit tests need to be updated** because we’re now excluding non-visible impressions. The new expected output needs to be manually crafted.

```python
# BEFORE - Unit test: Column level
def test_get_impress_position(logs):
    impress_events = logs.explode('impressions')
    impress_positions = get_impress_positions(impress_events)

    # Since each impress log has 4 items, expect impression positions to be five sets of 1-4
    pd.testing.assert_series_equal(impress_positions, pd.Series([1, 2, 3, 4] * 5))


# AFTER - Unit test: Column level (updated)
def test_get_impress_position(logs):
    logs = update_impression_col(logs)
    impress_events = logs.explode('impressions')
    impress_positions = get_impress_positions(impress_events)

    # Update impress positions (too brittle?)
    pd.testing.assert_series_equal(impress_positions, pd.Series([1, 2, 3,
                                                                 1, 2,
                                                                 1, 2, 3, 4,
                                                                 1, 2, 3, 4,
                                                                 1, 2]))
```

**Existing table-level unit tests need to be updated** because the filtered impressions affect aggregation results.

```python
# BEFORE - Unit test: Table level
def test_aggregate_events(events):
    result = aggregate_events(events)

    arr = [['i1', 0, 5],
           ['i2', 1, 5],
           ['i3', 1, 5],
           ['i4', 0, 5]]
    cols = ['item', 'click', 'impress']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)

    
# AFTER - Unit test: Table level (updated)
def test_aggregate_events(events):
    result = aggregate_events(events)

    arr = [['i1', 0, 4],  # Updated impress count
           ['i2', 1, 5],
           ['i3', 1, 4],  # Updated impress count
           ['i4', 0, 2]]  # Updated impress count
    cols = ['item', 'click', 'impress']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)
```

**Existing schema tests are unchanged.** Regardless of how we transform behavioral logs, the schema for the minimum set of columns should stay constant.

**Existing integration tests need to be updated** for the same reason why column/table-level unit tests need to be updated. Any updates to data or logic will affect intermediate data throughout the pipeline as well as the batch-inference output at the end of the pipeline.

```python
# BEFORE - Integration test: Aggregated events to batch inference
def test_model_pipeline(events, model, items):
    model = model.fit(events)
    result = model.batch_predict(items)

    arr = [['i1', 0.0],
           ['i2', 0.2],
           ['i3', 0.2],
           ['i4', 0.0],
           ['i5', -1.0]]
    cols = ['item_id', 'expected_ctr']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)


# AFTER - Integration test: Aggregated events to batch inference (updated)
def test_model_pipeline(events, model, items):
    model = model.fit(events)
    result = model.batch_predict(items)

    arr = [['i1', 0.0],
           ['i2', 0.2],
           ['i3', 0.25],  # Update expected CTR
           ['i4', 0.0],
           ['i5', -1.0]]
    cols = ['item_id', 'expected_ctr']
    expected = pd.DataFrame(arr, columns=cols)

    pd.testing.assert_frame_equal(result, expected)
```

To recap:

- Unit tests (row-level): Existing tests unchanged; add tests for new methods.
- Unit tests (col-level): Update existing tests.
- Unit tests (table-level): Update existing tests.
- Schema test: Existing tests unchanged; optionally add tests for new columns.
- Integration tests: Update existing tests.

Via this simple example, we see that **the effect of new data or logic is (i) additive for row-level unit tests and schema tests but (ii) retroactive for column/table-level unit tests and integration tests.** For the former, we simply add new tests while keeping existing tests unchanged. For the latter, we need to update most existing tests—they’re **brittle to changes in pipeline data and logic**.

Also, considering how the latter tests are more time-consuming to write and update, this explains why we spend more time on them than on writing feature code.

In the example pipeline and tests above, updating the tests wasn’t too onerous as we only had to update the code generating the input and expected output. Furthermore, the tables were small with few rows and columns. However, if we have more data in a format that’s less accessible in an IDE (e.g., Parquet), it would be a pain to update the input and expected output across many rows and columns.

**One way to address this is data generation and property-based testing.** Data generation involves generating fake data based on specifications such as numerical distributions and valid categorical values. Libraries such as [Faker](https://faker.readthedocs.io/en/master/) and [Hypothesis](https://hypothesis.readthedocs.io/en/latest/) help with this.

To apply property-based testing to our pipeline, we would (i) generate a lot of data, (ii) put the data through the pipeline, and (iii) check that the output meets the predefined properties. (In contrast, what we’ve been discussing is example-based testing: Given an input, check for the fixed output.) Libraries that support property-based testing include: [Hypothesis](https://hypothesis.readthedocs.io/en/latest/), [Pandera](https://pandera.readthedocs.io/en/stable/) (via Hypothesis and with additional schema checks), and [Giskard](https://github.com/Giskard-AI/giskard) (perturbations on text, image, and tabular input and tests on model variance).

Property-based testing seems like a promising approach. I used Faker for it a while back. Nonetheless, I’m uncertain about its ability to test column and table-level data transforms. Furthermore, property-based testing is [fuzzy](https://hypothesis.works/articles/what-is-property-based-testing/): We pass fuzzed, generated data to the code and test if it fails. While this helps to test edge cases at scale, I’m not convinced it can easily be applied to test for specific logic such as checking that items are ranked correctly before and after business logic (e.g., diversification, boosting).

If a test breaks and has to be updated frequently, is it a **valid** test? Maybe not. A test should protect against regression—if we have to change it often, it’s likely not working correctly.

Finding the right **granularity** for a test can be challenging but it makes testing more robust. By testing for exact values in the integration tests above, I overly coupled expected output with implementation details. Instead, I probably should have tested on more coarse-grained values such as the number of rows, columns, and unique item IDs. It would have made those integration tests more robust and not break when new data was added.

Finally, we should test as early as possible (i.e., at the smallest **scopes**) and in the most generic way. Row-level unit tests do this by considering only a single row’s input and output. With comprehensive row-level unit tests, we can rely less on integration tests.

## Many row-level and schema tests, a handful of the rest

To sum up, row-level unit tests and schema tests are less effort to write and maintain. They’re also robust to new data and business logic. Furthermore, having comprehensive row-level tests reduces reliance on higher-level tests. I tend to apply them generously.

In contrast, column/table-level unit tests and integration tests tend to be brittle. Finding the right granularity for a test can be challenging but makes testing more robust. Despite the downsides, these tests provide an irreplaceable safety harness though they should be used sparingly lest they become too cumbersome to maintain. I’ve come to depend on the assurance they provide but will probably use less of them in the future.

What’s your take on testing data or machine learning pipelines? What approaches have you found effective? I would love to hear from you!

## Further Reading

- [The Different Types of Software Testing](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing)
- [What are the Differences between Unit Tests, Integration Tests, Smoke Tests, etc?](https://stackoverflow.com/questions/520064/what-are-the-differences-between-unit-tests-integration-tests-smoke-tests-and)
- [Effective Testing for Spark Programs](https://www.slideshare.net/hkarau/effective-testing-for-spark-programs-strata-ny-2015)
- [The Mechanics of Testing Large Data Pipelines](https://www.slideshare.net/mathieu-bastian/the-mechanics-of-testing-large-data-pipelines-qcon-london-2016)
- [Test Strategies for Data Processing Pipelines](https://www.slideshare.net/lallea/test-strategies-for-data-processing-pipelines-67244458)
- [Engineering Data Quality](https://www.slideshare.net/lallea/engineering-data-quality)
- [Testing Features with PyTest](https://towardsdatascience.com/testing-features-with-pytest-82765a13a0e7)
- [Intro to Property-based Testing in Python](https://www.freecodecamp.org/news/intro-to-property-based-testing-in-python-6321e0c2f8b/)
- [What is Property Based Testing?](https://hypothesis.works/articles/what-is-property-based-testing/)
- [Testing in Microsoft Recommenders](https://github.com/microsoft/recommenders/tree/main/tests)

**Thanks** to [David Said](https://www.linkedin.com/in/david-said-martinez-3836a72b/), [Jim Dowling](https://twitter.com/jim_dowling), [Shreya Shankar](https://twitter.com/sh_reya), [Mike Lam](https://www.linkedin.com/in/mikeyplam/), and [Lars Albertsson](https://twitter.com/lalleal) for discussions on this topic. OG image by [Sam Loyd](https://unsplash.com/@samloyd) on [Unsplash](https://unsplash.com/s/photos/pipes).

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Sep 2022). Writing Robust Tests for Data & Machine Learning Pipelines. eugeneyan.com. https://eugeneyan.com/writing/testing-pipelines/.

or

```
@article{yan2022testing,
  title   = {Writing Robust Tests for Data & Machine Learning Pipelines},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2022},
  month   = {Sep},
  url     = {https://eugeneyan.com/writing/testing-pipelines/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
