# sftricksnstuff
Salesforce JS code and triggers


***************************    TRIGGERS *************************************



trigger UpdatePrimaryContact2 on Apttus__APTS_Agreement__c (before insert, before update) {

  //always set the customer to Diego Francis
  //trigger.new[0].Apttus__Primary_Contact__c='003U000000HDGgG';
       List<Contact> contacts = [select id, name from contact where name = 'Benjamin Brown'];
       trigger.new[0].Apttus__Primary_Contact__c = contacts[0].Id;
}


************************************************************************************************** JAVASCRIPTS *****************************************************************************************






***********************************ON CLICK JS CUSTOM  BUTTON TO UPDATE A FIELD *********************    D.STEIL   *****************)

{!REQUIRESCRIPT("/soap/ajax/29.0/connection.js")}
var AgrObj = new sforce.SObject("Apttus__APTS_Agreement__c");
AgrObj.Id = '{!Apttus__APTS_Agreement__c.Id}';
AgrObj.Apttus__Status__c = 'Shopping Cart Submitted';

// Add ^^^ with the field and the new value to update multiple values at once
var result = sforce.connection.update([AgrObj]);
location.reload(true);




************************************************************************ X-AUTHOR JAVASCRIPT LAUNCHER FROM BUTTON  *******************************   D.CLARK   ****************************************


{!REQUIRESCRIPT("/xdomain/xdomain.js")} 
{!REQUIRESCRIPT("/soap/ajax/26.0/connection.js")}
{!REQUIRESCRIPT("/support/console/28.0/integration.js")} 


var parentId = "{!Account.Id}";
var hostUrl = "https://na17.salesforce.com"; 
var appId = "69d39139-f830-4efa-9de5-5a0c13f9492c"
var sessionId = '{!GETSESSIONID()}';

var urlVal = ('xauthorforexcel:export '+ appId + ' ' + parentId + ' ' + sessionId + ' ' + hostUrl );

window.top.location.href  =urlVal;



********************************




**************************************************** JQUERY OPEN ANY SALESFORCE RECORD FROM A TAB***************************  B. XIA   ****************************************************


<apex:page >
    <apex:includeScript value="{!URLFOR($Resource.SDO_jqueryui192, '/js/jquery-1.8.3.js')}"/>
    <Script type="text/javascript">
	$( document ).ready(function(){
        window.location.href="https://apttuscommunity-14d539319ad.force.com/0011a000004K1M4";
        });
    </Script>
    <div align="center" >
    </div>
</apex:page>


***************************************

<apex:page >
    <div align="center">
    <img src="{!URLFOR($Resource.KeysightPortalHome)}" alt="Smiley face" onClick=" window.open('https://apttuscommunity-14d539319ad.force.com/001/o', '_blank', 'toolbar=0,location=0,menubar=0');  "/>
    </div>
    
</apex:page>
 
 
 
 
 
 
 ********************************************************   UPDATE STAGE APEX TRIGGER ********************** J. LEE 5/26/2015   **************************************************
 
 trigger updateStatus on Attachment (after insert) {
	List<Id> IdList = new List<Id>();
    List<Coin__c> CoinList = new List<Coin__c>();
    if(Trigger.new<>null){
        for(Attachment Att: Trigger.new){
            IdList.add(Att.ParentId);
        }
    }
    CoinList = [SELECT Id, ApttusESignatureStage__c
                FROM Coin__c C
                WHERE C.Id in: IdList];
    for(Coin__c coin : CoinList){
        coin.ApttusESignatureStage__c = 'Generated';
    }
    update CoinList;
}

 
 
 
 
 
**********************************************************************************LIST VIEW BUTTON THAT MASS MOVES RECORDS FROM ONE LIST VIEW TO ANOTHER ******************* D.STEIL **********************

{!REQUIRESCRIPT("/soap/ajax/19.0/connection.js")} 

var url = parent.location.href;

var records = {!GETRECORDIDS($ObjectType.Order__c)}; 
var updateRecords = []; 


if (records[0] == null) {
	alert("Please select at least one order to complete."); 
} else {

	for (var a=0; a<records.length; a++) { 
		var update_Order__c = new sforce.SObject("Order__c");
		update_Order__c.Id = records[a];
		update_Order__c.Completed__c = "TRUE";
		updateRecords.push(update_Order__c);
	}
	result = sforce.connection.update(updateRecords);

	parent.location.href = url;
}
