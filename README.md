# yii2-sortable-gridview
This is an extension of Yii2 GridView.

This extension render a sortable GridView which you can drag and drop the record items from the list, and store the order in ActiveRecord.

## Getting Started
### Installing
Install with Composer:

    composer require taimurkhan911/yii2-sortable-gridview "*"

or

    php composer.phar require taimurkhan911/yii2-sortable-gridview "*"

or add

    "taimurkhan911/yii2-sortable-gridview":"*"
to the require section of your composer.json file.

### Setting up SortableAction
In your controller, add the SortableAction into action():
```php
use taimurkhan911\sortable\SortableAction;

public function actions(){
    return [
        'sortItem' => [
            'class' => SortableAction::className(),
            'activeRecordClassName' => YourActiveRecordClass::className(),
            'orderColumn' => 'name_of_field_storing_ordering',
            'startPosition' => 1, // optional, default is 0
        ],
        // your other actions
    ];
}
```

### Setting up SortableGridView
In the view file, use SortableGridView as using the Yii default GridView
```php
use richardfan\sortable\SortableGridView;

<?= SortableGridView::widget([
    'dataProvider' => $dataProvider,
    
    // you can choose how the URL look like,
    // but it must match the one you put in the array of controller's action()
    'sortUrl' => Url::to(['sortItem']),
    
    'columns' => [
        // Data Columns
    ],
]); ?>
```

You may also want to disable the pagination of data provider, in order to allow reordering across pages.

You can do it by calling below before passing data provider into SortableGridView
```php
$dataProvider->pagination = false;
```
## Configurations
### SortableAction
example:
```php
use richardfan\sortable\SortableAction;

public function actions(){
    return [
        'sortItem' => [
            'class' => SortableAction::className(),
            'activeRecordClassName' => Articles::className(),
            'orderColumn' => 'sortOrder',
        ],
        // your other actions
    ];
}
```

* **activeRecordClassName**  (required)The ActiveRecrod class name. Use the full class name with namespace.
* **orderColumn**  (required) The column name which store the sorting order of each records. The column should be integer.

---

### SortableGridView
example:
```php
use taimurkhan911\sortable\SortableGridView;

<?= SortableGridView::widget([
    'dataProvider' => $dataProvider,
    
    // SortableGridView Configurations
    'sortUrl' => Url::to(['sortItem']),
    'sortingPromptText' => 'Loading...',
    'failText' => 'Fail to sort',
    'moveItem' => '.moveItem', 
    
    'columns' => [
        [
            'content' => function(){
                return "<span class='glyphicon glyphicon-resize-vertical'></span>";
            },
            'contentOptions' => ['style'=>'cursor:move;', 'class' => 'moveItem'],
        ],

        // Data Columns
    ],
]); ?>
```

* **sortUrl**  (required) The URL link to the SortableAction defined in controller's action().
* **sortingPromptText**  (optional) The text shown in the model while the server is reordering model. You can use HTML tag in this attribute. Default to "Loading...".
* **failText**  (optional) The text shown in alert box when sorting failed. Default to "Fail to sort".

## License
yii2-sortable-gridview is released under the MIT License. See the bundled LICENSE for details.
