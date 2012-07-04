SDNestedTables
===============

Obj-C module built on UITableView for creating a 2-level nested list UI control, iOS 4.0+.

![EFMNestedTables Screenshot 1 - iPhone Portrait](http://github.com/morekid/EFMNestedTables/raw/master/README/iPhone_P_shot1.png)&nbsp;&nbsp;
![EFMNestedTables Screenshot 2 - iPhone Portrait](http://github.com/morekid/EFMNestedTables/raw/master/README/iPhone_P_shot2.png)&nbsp;&nbsp;
![EFMNestedTables Screenshot 3 - iPhone Portrait](http://github.com/morekid/EFMNestedTables/raw/master/README/iPhone_P_shot3.png)&nbsp;&nbsp;
![EFMNestedTables Screenshot 4 - iPhone Portrait](http://github.com/morekid/EFMNestedTables/raw/master/README/iPhone_P_shot4.png)&nbsp;&nbsp;
![EFMNestedTables Screenshot 1 - iPad Landscape](http://github.com/morekid/EFMNestedTables/raw/master/README/iPad_L_shot1.png)


How To:
-------

Create a subclass of EFMNestedTable. You can call either init or initWithNibName:bundle:. Calling init will call initWithNibName:@"EFMNestedTable" bundle:nil. 

Then implement the following convenience methods:

<br />
### Filling the views




#### - (NSInteger)mainTable:(UITableView *)mainTable numberOfItemsInSection:(NSInteger)section;
here you can set the amount of Items in your Main table:

	- (NSInteger)mainTable:(UITableView *)mainTable numberOfItemsInSection:(NSInteger)section
	{
	    return 15; // amount of Main Items
	}
	
		
#### - (NSInteger)mainTable:(UITableView *)mainTable numberOfSubItemsforItem:(SDGroupCell *)item atIndexPath:(NSIndexPath *)indexPath;
here you can set the amount of Sub Items for each Item in the Main table:

	- (NSInteger)mainTable:(UITableView *)mainTable numberOfSubItemsforItem:(SDGroupCell *)item atIndexPath:(NSIndexPath *)indexPath
	{
	    return 3; // amount of Sub Items for each Main Item
	}
	

#### - (SDGroupCell *)mainTable:(UITableView *)mainTable setItem:(SDGroupCell *)item forRowAtIndexPath:(NSIndexPath *)indexPath;
here you can set the Item's cell attributes:

	- (SDGroupCell *)mainTable:(UITableView *)mainTable setItem:(SDGroupCell *)item forRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    item.itemText.text = [NSString stringWithFormat:@"My Main Item %u", indexPath.row +1];
	    return item;
	}


#### - (SDSubCell *)item:(SDGroupCell *)item setSubItem:(SDSubCell *)subItem forRowAtIndexPath:(NSIndexPath *)indexPath;
here you can set the Sub Item's cell attributes:

	- (SDSubCell *)item:(SDGroupCell *)item setSubItem:(SDSubCell *)subItem forRowAtIndexPath:(NSIndexPath *)indexPath
	{
	    subItem.itemText.text = [NSString stringWithFormat:@"My Sub Item %u", indexPath.row +1];
	    return subItem;
	}


<br />
### Delegate methods

EFMNestedTable implements the EFMNestedTableDelegate protocol, however you are free to implement the following methods yourself if they will provide useful information.

#### - (void)mainTable:(UITableView *)mainTable itemDidChange:(SDGroupCell *)item;
this is called when the Item state changes, here you can manage behavior according to the Item state:

	- (void)mainTable:(UITableView *)mainTable itemDidChange:(SDGroupCell *)item
	{
		SelectableCellState state = item.selectableCellState;
		NSIndexPath *indexPath = [self.tableView indexPathForCell:item];
	    switch (state) {
	        case Checked:
	        	// do stuff
	            break;
	        case Unchecked:
	        	// do stuff
	            break;
	        case Halfchecked:
	        	// do stuff
	            break;
	        default:
	        break;
	    }
	}
	

#### - (void) item:(SDGroupCell *)item subItemDidChange:(SDSelectableCell *)subItem;
this is called when the Sub Item state changes, here you can manage behavior according to the Sub Item state:

	- (void) item:(SDGroupCell *)item subItemDidChange:(SDSelectableCell *)subItem
	{
	    SelectableCellState state = subItem.selectableCellState;
	    NSIndexPath *indexPath = [item.subTable indexPathForCell:subItem];
	    switch (state) {
	        case Checked:
	        	// do stuff
	            break;
	        case Unchecked:
	        	// do stuff
	            break;
	        default:
	            break;
	    }
	}


<br />
### Other methods

#### - (void)expandingItem:(SDGroupCell *)item withIndexPath:(NSIndexPath *)indexPath;
this is called when a Main Item starts collapsing, here you can manage behavior according to this event:

	- (void)expandingItem:(SDGroupCell *)item withIndexPath:(NSIndexPath *)indexPath
	{
		// do stuff
	}


#### - (void)collapsingItem:(SDGroupCell *)item withIndexPath:(NSIndexPath *)indexPath;
this is called when a Main Item starts expanding, here you can manage behavior according to this event:

	- (void)collapsingItem:(SDGroupCell *)item withIndexPath:(NSIndexPath *)indexPath 
	{
		// do stuff
	}


<br />
### Table Items properties


#### Main Items & Sub Items

SDNestedTableViewController *parentTable

SelectableCellState selectableCellState

UILabel *itemText


#### Main Items Only

UITableView *subTable

BOOL isExpanded


	// all items
	
	item.parentTable
	
	item.selectableCellState
	
	item.itemText
	
	
	// only Main Items
	
	item.subTable
	
	item.isExpanded


Roadmap/Feature Ideas
-------

Can't use custom classes cleanly in place of the default cells

Cell heights are tightly coupled to default classes

Provide more delegate methods

Pass "tapped" event info to the delegate methods in order to know if the Item changed state passively or actively
