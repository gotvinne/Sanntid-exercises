
# Synchronization in Ada: 

Synchronization is done by defining protected objects. Waiting from the inside is illegal. Procedures and entries are mutual excluded 
*Ada is generally more readable due to guards and entires* 

General Ada syntax: 
```
//Declare object
protected 'object_name' is 
    entry func1;
    ..
private
    shared variables;
end 'object_name';



//Implement object
protected body "object_name" is

    private int variable = 0;//Declare shared variables 

    entry entry_name(argument_variable : Argument_Type) when guard is 
    begin 
        //entry;
    end entry_name;

    procedure procedure_name is 
    begin 
        //change variable;
    end procedure_name;

    function function_name is 
    begin 
        //read only 
    end function_name;
end object_name;
```

Threading in Ada is a while True


Bounded buffer: 

```
protected body Bounded_Buffer is

    entry Get (Item : out Data_Item) when Num /= 0 is
    begin
        Item := Buf(First);
        First := First + 1;
        Num:=Num-1;
    end Get;


    entry Put (Item : in Data_Item) when Num /= Buffer_Size is
    begin
        Last := Last + 1; 
        Num := Num + 1;
        Buf(Last) := Item
    end Put;
    end Bounded_Buffer;

```

Barrier Ada: //three threads 
--------------------------

```
protected body Barrier is

    private int count = 0;

    entry increment when not valid is 
    begin 
        count++;
        valid = true; 
    end increment;

    entry continue when count == 3 is 
    begin 
        //entry after barrier;
    end continue; 
end Barrier
```