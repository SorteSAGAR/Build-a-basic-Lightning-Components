.html:
<template>
    <lightning-card title=" Creted Accounts">
        <div class="slds-m-around_medium">
            <template if:true={accounts}>
                <ul class="slds-list_dotted">
                    <template for:each={accounts} for:item="account">
                        <li key={account.Id}>{account.Name}</li>
                    </template>
                </ul>
            </template>
            <template if:false={accounts}>
                No accounts found.
            </template>
        </div>
    </lightning-card>
</template>

js:
import { LightningElement, wire } from 'lwc';
import getRecentlyCreatedAccounts from '@salesforce/apex/AccountController.getRecentlyCreatedAccounts';

export default class RecentlyCreatedAccounts extends LightningElement {
    accounts;

    @wire(getRecentlyCreatedAccounts)
    wiredAccounts({ error, data }) {
        if (data) {
            this.accounts = data;
        } else if (error) {
            console.error(error);
        }
    }
}


.js-meta.xml
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>53.0</apiVersion>
    <isExposed>true</isExposed>
</LightningComponentBundle>



Apex code --

public with sharing class AccountController {
    @AuraEnabled(cacheable=true)
    public static List<Account> getRecentlyCreatedAccounts() {
        return [SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}

