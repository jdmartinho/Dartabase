Dartabase Model 0.6.3
=====================

  Serverside Database Object Models for simple data manipulation
  using MySQL/PGSQL without having to write SQL
  
  inspired by Ruby on Rails models
    
  This requires the use of [Dartabase Migration](http://pub.dartlang.org/packages/dartabase_migration) 
    
	Tested on 

    Dart Editor version 1.4.0.dev_06_03 (DEV)
    Dart SDK version 1.4.0-dev.6.3
    
	Compatibility
		depending on the migration version you are using 
		you have to use a differend model version in your app
	    
	    migration  			model
	    -------------------------
	    0.6.x <-requires->  0.6.x
      0.5.x <-requires->  0.5.x
	    
  Uses
  
  MYSQL via [sqljocky](http://pub.dartlang.org/packages/sqljocky) version 0.11.0
  
  PGSQL via [postgresql](http://pub.dartlang.org/packages/postgresql) version 0.2.13
  
**TUTORIAL 1** [HOW TO SETUP AND RUN MIGRATION AND MODEL](https://github.com/HannesRammer/DartabaseTutorials/blob/master/tutorials/TUT1.md)
    	
HOW TO SETUP
------------

After you have sucessfully finished setting up 'Dartabase Migration' 

1. Install Dartabase Model the usual pubspec way 
    
2. Inside your project, at the beginning of the main method insert
        
		Model.initiate("path-to-your-project");

   now it should look kinda like this:
	
		-----dataserver.dart--START--
	
		library dataServer;

		import 'package:dartabase_model/dartabase_model.dart';

		main(){
		  Model.initiate("C:\\darttestproject\\DartabaseServer");
		  ... your code
		}
	
		-----dataserver.dart--END--
	
3. Imagine you have ONLY created one database table named 'account' 

    	with the column 'name'
	
4. You have to extend all classes that you want to connected to the database

	    with 'Model'
		   
		in this case we create a class Account with id, name and a counter
		   
		-----account.dart--START--
			
		part of dataServer;
		
		class Account extends Model{
		  num id;		
		  String name;
		  num counter;
		}
		
		-----account.dart--END--

5. Now add account.dart as part to dataServer so you can access Account
   (obviously when you have everything in the same file,
   you dont need 'part' and 'part_of') 
	
		-----dataserver.dart--START--
			
		library dataServer;
	
		import 'package:dartabase_model/dartabase_model.dart';
		part "account.dart";	
		
		main(){
		  Model.initiate("C:\\darttestproject\\DartabaseServer");
		  ... your code
		}
	
		-----dataserver.dart--END--
 

*******************************************************************************************
HOW TO USE
----------

SIMPLE MODEL FUNCTIONS
----------------------

**Future save()** 
    
    once future completes
     
    Returns String "created" or "updated"
    
    player.save().then((process){
      if(process == "created" || process == "updated"){
        //your code
      }else{
      }
    }); 
   
**Future findBy(String column,var value)** 
    
    once future completes
    
    returns an (player) object if one exists 
    else 
    returns null
   
    player.findBy("name","tim").then((player){
      if(player != null){
        //your code
      }else{
      }
    }); 
    
**Future findById(var id)** 
    
    once future completes
    
    accepted type of id is (String || int || num)
     
    returns an (player) object if one exists 
    else 
    returns null
   
    player.findById("3").then((player){
      if(player != null){
        //your code
      }else{
      }
    }); 
    
**Future findAllBy(String column, var value)** 
    
    once future completes
    
    returns a list of (player) objects if one exists 
    else 
    returns empty list
   
    player.findAllBy("name","tim").then((players){
      if(!players.isEmpty){
        //your code
      }else{
      }
    }); 
 
**Future findAll()** 
    
    once future completes
    
    returns a list of all (player) objects if one exists 
    else 
    returns empty list
   
    player.findAll().then((players){
      if(!players.isEmpty){
        //your code
      }else{
      }
    }); 
 
**Future delete()** 
    
    once future completes
    
    deletes the object //TODO and all its relations
    
    player.delete();
    
    
RELATIONS
---------

**Future receive(object)** 
	 
	once future completes
	creates relation between the two objects (player and character)
	...
	    
	player.receive(character).then((result){
	      
	}); 
    
**Future hasOne(object)** 
   
	once future completes
	  
	returns an (character) object if one exists 
	else 
	returns null
	   
	player.hasOne(new Character()).then((character){
	  if(character != null){
	    //your code
	  }else{
	  }
	};
  
**Future hasMany(object)** 
    
    once future completes
    
    returns a list of (character) objects if one exists 
    else 
    returns empty list
   
    player.hasManyWith(new Character()).then((characters){
      if(!characters.isEmpty){
        //your code
      }else{
      }
    }); 
  
**Future hasOneWith(object,String column,String value)** 
	    
	once future completes
	 
	returns an (character) object if one exists 
	else 
	returns null
	 
	player.hasOneWith(new Character(),'level','3').then((character){
	  if(character != null){
	    //your code
	  }else{
	  }
	});

**Future hasManyWith(object,String column,String value)** 
	
	once future completes
	 
	Returns a list of (character) objects if one exists 
	else 
	Returns empty list
	   
	player.hasManyWith(new Character(),'level','3').then((characters){
	  if(!characters.isEmpty){
	    //your code
	  }else{
	  }
	}); 
  
**Future remove(object)** 
	
	once future completes
	remove relation between the two objects (player and character)
	...
	
	player.remove(character).then((result){
	  
	}); 

**TEST EXAMPLE**
say we have a database with table account and table picture 
	
		-----dataserver.dart--START--
	
			library gameServer;
	
			import 'package:dartabase_model/dartabase_model.dart';
			import 'dart:async';
			part "account.dart";
			part "picture.dart";
			
			void main() {
			  Model.initiate("C:\\darttestproject\\gameServer");
			  testAll();
			}
			
			Future testAll() {
			  Completer completer = new Completer();
			  save().then((List objects){
			    print("save DONE");
			    find().then((_){
			      print("find DONE");
			      receive(objects[0],objects[1]).then((_){
			        print("receive DONE");
			        has(objects[0],objects[1]).then((_){
			          print("has DONE");
			          remove(objects[0],objects[1]).then((_){
			            print("remove DONE");
			            delete(objects[0],objects[1]).then((_){
			              print("delete DONE");
			              print("2testAll DONE");
			            });
			          });
			        });
			      });
			    });
			  });
			  return completer.future;
			}
			
			/**
			 * save test data into db
			 * 
			 * save account 1
			 * save account 2
			 * save picture 1
			 * save picture 2
			 * save picture 3
			 **/
			Future save() {
			  Completer completer = new Completer();
			  Account account1 = new Account();
			  
			  account1.username="testUser1";
			  account1.name="guest";
			  account1.id=1; // on empty db should save user.id =1
			  account1.save().then((_){
			    print('account1 saved : ${account1.toString()}');
			    Account account2 = new Account();
			    account2.username="testUser2";
			    account2.name="guest";
			    account2.id=2; // on empty db should save user.id =2
			    account2.save().then((_){
			      print('account2 saved : ${account2.toString()}');
			      Picture picture1 = new Picture();
			      picture1.filename="profile";
			      picture1.id=1;
			      picture1.save().then((_){
			        print('picture1 saved : ${picture1.toString()}');
			        Picture picture2 = new Picture();
			        picture2.id=2;
			        picture2.filename="profile";
			        picture2.save().then((_){
			          print('picture2 saved : ${picture2.toString()}');
			          Picture picture3 = new Picture();
			          picture3.id=3;
			          picture3.filename="profile";
			          picture3.save().then((_){
			            print('picture3 saved : ${picture3.toString()}');
			            print("testdata entered into DB");
			            print("fillDB() DONE!!!");
			            completer.complete([[account1,account2],[picture1,picture2,picture3]]);
			            
			          });
			        });
			      });
			    });
			  });
			  return completer.future;
			}
			 
			/**
			 * find test data from db
			 * 
			 * account findBy username testUser
			 * account findById 2
			 * account findAllBy name guest
			**/
			Future find() {
			  Completer completer = new Completer();
			  Account accountSearch = new Account();
			  accountSearch.findBy("username", "testUser1").then((account1){
			    print('accountSearch.findBy("username", "testUser1") done. Result:');
			    print(account1.toString());
			    accountSearch.findById(2).then((Account account2){
			      print('accountSearch.findById(2) done. Result:');
			      print(account2.toString());
			      accountSearch.findAllBy("name", "guest").then((List accounts){
			        print('accountSearch.findAllBy("name", "guest") done. Results:');
			        for(num i = 0;i<accounts.length;i++){
			          print(accounts[i].toString());
			        }
			        print("find() DONE!!!");
			        completer.complete(true);
			      });
			    });
			  });
			  return completer.future;
			}
			
			/**
			 * creates relation between data in db
			 * 
			 * account 1 receive picture 1
			 * account 2 receive picture 2
			 * account 2 receive picture 3
			**/
			Future receive(List<Account> accounts,List<Picture> pictures) {
			  Completer completer = new Completer();
			  accounts[0].receive(pictures[0]).then((_){
			    print('account${accounts[0].id} received picture${pictures[0].id}');
			    accounts[1].receive(pictures[1]).then((_){
			      print('account${accounts[1].id} received picture${pictures[1].id}');
			      accounts[1].receive(pictures[2]).then((_){
			        print('account${accounts[1].id} received picture${pictures[2].id}');
			        print("receive() DONE!!!");
			        completer.complete(true);
			      });
			    });
			    
			  });
			  return completer.future;
			}
			
			/**
			  * finds relation between data in db
			  * 
			  * account 1 hasOne picture
			  * account 1 hasOneWith picture filename profile
			  * account 2 hasMany picture
			  * account 2 hasManyWith picture filename profile
			**/
			Future has(List<Account> accounts,List<Picture>  pictures) {
			  Completer completer = new Completer();
			  accounts[0].hasOne(new Picture()).then((picture){
			    print('accounts[0].hasOne(new Picture()) done. Result:');
			    print(picture.toString());
			    accounts[0].hasOneWith(new Picture(),"filename","profile").then((picture){
			      print('accounts[0].hasOneWith(new Picture(),"filename","profile")account done. Result:');
			      print(picture.toString());
			      accounts[1].hasMany(new Picture()).then((List pictures){
			        print('accounts[1].hasMany(new Picture()) done. Results:');
			        for(num i = 0;i<pictures.length;i++){
			          print(pictures[i].toString());
			        }
			        accounts[1].hasManyWith(new Picture(),"filename","profile").then((List pictures){
			          print('accounts[1].hasManyWith(new Picture(),"filename","profile") done. Results:');
			          for(num i = 0;i<pictures.length;i++){
			            print(pictures[i].toString());
			          }
			          print("has() DONE!!!");
			          completer.complete(true);
			        });
			      });
			    });
			  });
			  return completer.future;
			}
			
			/**
			 * remove picture 2 from account 2 
			**/
			Future remove(List<Account> accounts,List<Picture>  pictures) {
			  Completer completer = new Completer();
			  accounts[1].remove(pictures[1]).then((_){
			    print('removed relation from picture.id = 2 and account.id = 2');
			    print("remove() DONE!!!");
			    completer.complete(true);
			  });
			  return completer.future;
			}
			
			/**
			 * delete account 1  
			**/
			Future delete(List<Account> accounts,List<Picture>  pictures) {
			  Completer completer = new Completer();
			  accounts[0].delete().then((_){
			    print('removed all relations for account.id = ${accounts[0].id}');
			    print('removed object account.id = ${accounts[0].id}');
			    print("delete() DONE!!!");
			    completer.complete(true);
			  });
			  return completer.future;
			}
				
		-----dataserver.dart--END--
		  
	  
	  	

*******************************************************************************************

TODO
----

	*wait for 'await' to make this baby sync
	*test functionality in bigger project
    *add more features like implementing and removing dependencies
    *add examples code to git
    *add automated tests
    *and much more

Please let me know about bugs you find and or improvements/features you would like to see in future.

ENJOY & BE NICE ;)