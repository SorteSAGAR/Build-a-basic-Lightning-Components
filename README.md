.html:
<template>
    <lightning-card title=" Creted accounts">
        <template if:true={accountrecords}>
             <template for:each={accountrecords} for:item="acc">
                <p key={acc.id}>{acc.Name}
                </p>    
            </template>
        </template>
    </lightning-card>
</template>



js:

 import { LightningElement,wire} from 'lwc';
import getRecently from '@salesforce/apex/CretedController.getRecently';

//import  getRecently from '@salesforce/apex/CretedController.getRecently';
export default class CretedAccount extends LightningElement {
    accountrecords;
   @wire(getRecently)
     wiredAccounts({ error, data }) {
        if (data) {
     //       this.accounts = data;
     console.log('Records',JSON.stringify(data));
     this.accountrecords=data;
        } else if (error) {
          //  console.error('Error loading recently created accounts:', error);

        }
    }
}


xml:

<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>58.0</apiVersion>
        <isExposed>true</isExposed>
    <targets>
   <target>lightning__AppPage</target>
    <target>lightning__HomePage</target>
  </targets>
</LightningComponentBundle>

 



Apex code --
public class CretedController {
  @AuraEnabled(cacheable=true)
    public static List<Account>getRecently() {
        return [SELECT Id, Name, CreatedDate FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}

