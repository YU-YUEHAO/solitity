pragma solidity ^0.5.0;
pragma experimental ABIEncoderV2;

contract PersonDID {
    
    struct Person {
        uint8 id;
        uint8 age;
        string name;
        string inself;
      
    }
     enum ErrorNo {
        NO_ERROR,
        BAD_PARAMETER,
        MOBILE_EMPTY,
        USER_NOT_EXISTS,
        MOBILE_ALREADY_EXISTS,
        ACCOUNT_ALREDY_EXISTS,
        NO_PERMISSION
    }


​    
```sol
event AddPerson(uint8 id, uint8 age, string name, uint timestamp);

Person admin;
Person[] persons;
mapping(address => Person) public PersonInfo;
mapping(address=> bool) public isPersonExsist;

constructor (address ip, uint8 id, string memory name, uint8 age,string memory inself) public {
    admin = Person(id, age, name,inself);
    Person memory p = Person(id, age, name,inself);
    persons.push(p);
    PersonInfo[msg.sender] = p;
    isPersonExsist[msg.sender] = true;
}

function getNumberOfPersons() view public returns (uint256) {
    return persons.length;
}

function addPerson(uint8 id, uint8 age, string memory name,string memory inself) public returns (bool) {
    require(!((id == 0) || age == 0), "persons info can not be empty!!");
    require(!isPersonExsist[msg.sender], "person can not exsist !!");
    Person memory person = Person(id, age, name,inself);
    persons.push(person);
    PersonInfo[msg.sender] = Person(id, age, name,inself);
    isPersonExsist[msg.sender] = true;
    emit AddPerson(id, age, name, now);
}

function setPersonAgeSto(address ip, uint8 age) public {
    Person storage p = PersonInfo[ip];
    p.age = age;
}

function setPersonAgeMem(address ip, uint8 age) public {
    Person memory p = PersonInfo[ip];
    p.age = age;
}

function getPersonAge(address ip) public view returns (uint8) {
    return PersonInfo[ip].age;
}
function alterinself(uint8 id, uint8 age, string memory name,string memory inself,address ip) public returns(uint8){
    if (tx.origin !=msg.sender){
        return 0;
    }else{
         Person memory p = PersonInfo[ip];
        p.inself =inself;
    }
}
```

}