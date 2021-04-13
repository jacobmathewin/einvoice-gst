<template>
	<div class="container-fluid">
		<h4 class="text-primary text-center p-2">
			E-Invoice & Eway Bill JSON Generator
		</h4>
		<div class="row text-primary justify-content-center">
			Transaction Details
		</div>
		<div class="row">
			<div class="col-6 text-right">
				Supply Type
			</div>
			<div class="col-6">
				<b>B2B</b>
			</div>
		</div>

		<div class="row text-primary justify-content-center mt-3">
			Document Details
		</div>

		<div class="row">
			<div class="col-6 text-right pt-2">
				Invoice No.
			</div>
			<div class="col-2">
				<input type="text" class="form-control text-center font-weight-bold" v-model="invoiceNo" />
			</div>
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Invoice Date
			</div>
			<div class="col-2">
				<date-picker v-model="invoiceDate" format="DD-MM-YYYY" input-class="text-center form-control font-weight-bold"></date-picker>
				
			</div>
		</div>
		
		<div class="row text-primary justify-content-center mt-3">
			Seller Details
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right">
				Seller
			</div>
			<div class="col-6">
				<b>32AAACT8234C1ZS (Canara)</b>
			</div>
		</div>
		
		<div class="row text-primary justify-content-center mt-3">
			Buyer Details
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer GSTIN
			</div>
			<div class="col-2">
				<input type="text" class="form-control text-center font-weight-bold" v-model="buyerGSTIN" />	
			</div>
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer Legal Name
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="buyerLegalName" />	
			</div>
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer Address
			</div>
			<div class="col-4">
				<textarea name="" id="" cols="30" rows="3" class="form-control font-weight-bold" v-model="buyerAddress"></textarea>
			</div>
		</div>
		

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer Location
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="buyerLocation" />	
			</div>
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer PIN
			</div>
			<div class="col-2">
				<input type="text" class="form-control font-weight-bold" v-model="buyerPIN" />	
			</div>
		</div>

		<div class="row pt-1">
			<div class="col-6 text-right pt-2">
				Buyer State
			</div>
			<div class="col-1 pt-2 text-center font-weight-bold">{{buyerState}}</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="buyerStateName" @input="buyerStateSearch" />	
			</div>
		</div>

		<div class="row">
			<div class="col-4 offset-7">
				<ul>
					<li v-for="stateCode,state in buyerStateSuggestions" v-bind:key="stateCode">
						<a @click="selectBuyerState(stateCode,state)" href="#buyerState">{{state}}</a>
					</li>
				</ul>
			</div>
		</div>
		
		<div class="row text-primary mt-3">
			<div class="col-6 text-right">
				Consignee Details
			</div>
			<div class="col-4 float-left">
				<input type="checkbox" v-model="enableConsignee" />
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee GSTIN
			</div>
			<div class="col-2">
				<input type="text" class="form-control text-center font-weight-bold" v-model="consigneeGSTIN" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Legal Name
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="consigneeLegalName" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Address
			</div>
			<div class="col-4">
				<textarea name="" id="" cols="30" rows="3" class="form-control font-weight-bold" v-model="consigneeAddress"></textarea>
			</div>
		</div>
		

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Location
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="consigneeLocation" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee PIN
			</div>
			<div class="col-2">
				<input type="text" class="form-control font-weight-bold" v-model="consigneePIN" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee State
			</div>
			<div class="col-1 pt-2 text-center font-weight-bold">{{consigneeState}}</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" v-model="consigneeStateName" @input="consigneeStateSearch" />	
			</div>
		</div>

		<div class="row">
			<div class="col-4 offset-7">
				<ul>
					<li v-for="stateCode,state in consigneeStateSuggestions" v-bind:key="stateCode">
						<a @click="selectConsigneeState(stateCode,state)" href="#consigneeState">{{state}}</a>
					</li>
				</ul>
			</div>
		</div>


		<div class="row text-primary mt-3">
			<div class="col-6 text-right">
				EWay Bill Details
			</div>
			<div class="col-4 float-left">
				<input type="checkbox" v-model="enableEwayBill" />
			</div>
		</div>

		<div class="row pt-1" v-show="enableEwayBill">
			<div class="col-6 text-right pt-2">
				Distance
			</div>
			<div class="col-2">
				<input type="text" class="form-control text-center font-weight-bold" v-model="ewayBillDistance" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableEwayBill">
			<div class="col-6 text-right pt-2">
				Transport Doc. No.
			</div>
			<div class="col-2">
				<input type="text" class="form-control font-weight-bold text-center" v-model="ewayBillTransportDocNo" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableEwayBill">
			<div class="col-6 text-right pt-2">
				Transport Doc. Date
			</div>
			<div class="col-2">
                <date-picker v-model="ewayBillTransportDocDate" format="DD-MM-YYYY" input-class="text-center form-control font-weight-bold"></date-picker>
			</div>
		</div>   

		<div class="row pt-1" v-show="enableEwayBill">
			<div class="col-6 text-right pt-2">
				Vehicle No.
			</div>
			<div class="col-2">
				<input type="text" class="form-control font-weight-bold text-center" v-model="ewayBillVehicleNo" />	
			</div>
		</div>        
		
		<div class="row text-primary mt-3">
			<div class="col-6 text-right">Item Details</div>
			<div class="col-4">
				<button class="btn btn-outline-primary btn-sm" v-on:click="addItem()">Add Item</button>
			</div>
		</div>

		<div class="row text-center font-weight-bold">
			<div class="col-1">S.No.</div>
			<div class="col-1">Description</div>
			<div class="col-1">HSN Code</div>
			<div class="col-1">Qty (kg)</div>
			<div class="col-1">Unit Price</div>
			<div class="col-1">GST Rate</div>
			<div class="col-1">Value</div>
			<div class="col-1">CGST</div>
			<div class="col-1">SGST</div>
			<div class="col-1">IGST</div>
			<div class="col-2">Total</div>
		</div>

		<div class="row text-center mt-2" v-for="item,key in itemsList" :key="key">
			<div class="col-1 smallText">{{item.SlNo}}</div>
			<div class="col-1"><input class="form-control smallText" v-model="item.PrdDesc" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.HsnCd" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.Qty" @input="calculateSubTotal(key)" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.UnitPrice" @input="calculateSubTotal(key)" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.GstRt" @input="calculateSubTotal(key)" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.TotAmt" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.CgstAmt" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.SgstAmt" /></div>
			<div class="col-1"><input class="form-control text-center smallText" v-model="item.IgstAmt" /></div>
			<div class="col-2"><input class="form-control text-center smallText" v-model="item.TotItemVal" /></div>
		</div>

		<div class="row text-center mt-2">
			<div class="col-2 offset-10"><input class="form-control text-center smallText font-weight-bold" v-model="ValDtls.TotInvVal" /></div>
		</div>

		<div class="row justify-content-center pt-3 mb-5">
			<button type="button" class="btn btn-outline-primary" v-on:click="saveFile()">Download JSON</button>
		</div>
		
	</div>
</template>

<script>

import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import moment from 'moment';

export default {
	name: 'MainComponent',
	components: {DatePicker},

	data: function() {
		return {
			items: [],
			invoiceNo: '',
			invoiceDate: new Date(),
			buyerGSTIN: '',
			buyerLegalName: '',
			buyerAddress: '',
			buyerLocation: '',
			buyerPIN: '',
			buyerState: '32',
			buyerStateName: 'Kerala',
			buyerStateSuggestions: '',
			enableConsignee: false,
            itemsList: [],

            enableEwayBill: false,
            ewayBillDistance: '',
            ewayBillTransportDocNo: '',
            ewayBillTransportDocDate: '',
            ewayBillVehicleNo: '',

            consigneeGSTIN: '',
            consigneeLegalName: '',
            consigneeAddress: '',
            consigneeLocation: '',
            consigneePIN: '',
            consigneeState: '',
            consigneeStateName: '',

            ValDtls:{
               "AssVal":0,
               "IgstVal":0,
               "CgstVal":0,
               "SgstVal":0,
               "CesVal":0,
               "StCesVal":0,
               "Discount":0,
               "OthChrg":0,
               "RndOffAmt":0,
               "TotInvVal":0
			},
			statesList: {
				'JAMMU AND KASHMIR': 1,
				'HIMACHAL PRADESH':	2,
				'PUNJAB': 3,
				'CHANDIGARH': 4,
				'UTTARAKHAND': 5,
				'HARYANA': 6,
				'DELHI': 7,
				'RAJASTHAN': 8,
				'UTTAR PRADESH': 9,
				'BIHAR': 10,
				'SIKKIM': 11,
				'ARUNACHAL PRADESH': 12,
				'NAGALAND': 13,
				'MANIPUR': 14,
				'MIZORAM': 15,
				'TRIPURA': 16,
				'MEGHLAYA': 17,
				'ASSAM': 18,
				'WEST BENGAL': 19,
				'JHARKHAND': 20,
				'ODISHA': 21,
				'CHATTISGARH': 22,
				'MADHYA PRADESH': 23,
				'GUJARAT': 24,
				'DADRA AND NAGAR HAVELI AND DAMAN AND DIU (NEWLY MERGED UT)': 26,
				'MAHARASHTRA': 27,
				'ANDHRA PRADESH(BEFORE DIVISION)': 28,
				'KARNATAKA': 29,
				'GOA': 30,
				'LAKSHWADEEP': 31,
				'KERALA': 32,
				'TAMIL NADU': 33,
				'PUDUCHERRY': 34,
				'ANDAMAN AND NICOBAR ISLANDS': 35,
				'TELANGANA': 36,
				'ANDHRA PRADESH (NEWLY ADDED)': 37,
				'LADAKH (NEWLY ADDED)': 38
			}
		}
	},

	props: {
	},

	methods: {

		buyerStateSearch() {
			let tempList = {};
			for(let key in this.statesList) {
				if(key.indexOf(this.buyerStateName.toUpperCase())!=-1)
					tempList[key] = this.statesList[key];
			}
			this.buyerStateSuggestions = tempList;
		},

		selectBuyerState(stateCode, state) {
			this.buyerState = stateCode;
			this.buyerStateName = state;
			this.buyerStateSuggestions = {};
		},

		consigneeStateSearch() {
			let tempList = {};
			for(let key in this.statesList) {
				if(key.indexOf(this.consigneeStateName.toUpperCase())!=-1)
					tempList[key] = this.statesList[key];
			}
			this.consigneeStateSuggestions = tempList;
		},

		selectConsigneeState(stateCode, state) {
			this.consigneeState = stateCode;
			this.consigneeStateName = state;
			this.consigneeStateSuggestions = {};
		},

		calculateSubTotal(key) {
            this.itemsList[key].TotAmt = (this.itemsList[key].Qty * this.itemsList[key].UnitPrice).toFixed(2);
            this.itemsList[key].TotAmt = parseFloat(this.itemsList[key].TotAmt);
            this.itemsList[key].AssAmt = this.itemsList[key].TotAmt;
            
            this.itemsList[key].CgstAmt = 0;
            this.itemsList[key].SgstAmt = 0;
            this.itemsList[key].IgstAmt = 0;

			if(this.buyerState==32) {
				this.itemsList[key].CgstAmt = (this.itemsList[key].TotAmt * this.itemsList[key].GstRt / 100 / 2).toFixed(2);
                this.itemsList[key].CgstAmt = parseFloat(this.itemsList[key].CgstAmt);
                this.itemsList[key].SgstAmt = this.itemsList[key].CgstAmt;
			} else {
                this.itemsList[key].IgstAmt = (this.itemsList[key].TotAmt * this.itemsList[key].GstRt / 100).toFixed(2);
                this.itemsList[key].IgstAmt = parseFloat(this.itemsList[key].IgstAmt);
			}
			this.itemsList[key].TotItemVal = this.itemsList[key].AssAmt + this.itemsList[key].SgstAmt + this.itemsList[key].CgstAmt + this.itemsList[key].IgstAmt;
            this.itemsList[key].TotItemVal = parseFloat(this.itemsList[key].TotItemVal).toFixed(2);
            this.calculateTotal();
        },
        
        calculateTotal() {
            this.ValDtls.AssVal = 0;
            this.ValDtls.CgstVal = 0;
            this.ValDtls.SgstVal = 0;
            this.ValDtls.IgstVal = 0;
            this.ValDtls.TotInvVal = 0;
            for(let key in this.itemsList) {
                this.ValDtls.AssVal += parseFloat(this.itemsList[key].AssAmt);
                this.ValDtls.CgstVal += parseFloat(this.itemsList[key].CgstAmt);
                this.ValDtls.SgstVal += parseFloat(this.itemsList[key].SgstAmt);
                this.ValDtls.IgstVal += parseFloat(this.itemsList[key].IgstAmt);
                this.ValDtls.TotInvVal += parseFloat(this.itemsList[key].TotItemVal);
            }
        },

		getFormattedDate(date) {
			return moment(date).format("DD/MM/YYYY");
		},

		saveFile: function() {

            // clone item details
            let tempItemsList = this.itemsList.slice(0);
            for(let key in tempItemsList) {
                tempItemsList[key].UnitPrice = parseFloat(tempItemsList[key].UnitPrice);
                tempItemsList[key].TotItemVal = parseFloat(tempItemsList[key].TotItemVal);
                tempItemsList[key].Qty = parseFloat(tempItemsList[key].Qty);
                tempItemsList[key].GstRt = parseFloat(tempItemsList[key].GstRt);
            }

			let jsonData = [{
			Version: "1.1",
		
			"TranDtls":{
				"TaxSch":"GST",
				"SupTyp":"B2B",
				"IgstOnIntra":"N",
				"RegRev":null,
				"EcmGstin":null
			},

			"DocDtls":{
				"Typ":"INV",
				"No": this.invoiceNo,
				"Dt": this.getFormattedDate(this.invoiceDate)
			},      
			
			"SellerDtls":{
               "Gstin":"32AAACT8234C1ZS",
               "LglNm":"The Canara Paper Mills Pvt. Ltd.",
               "TrdNm":"The Canara Paper Mills Pvt. Ltd.",
               "Addr1":"Chethipuzha, Kurishummoodu PO, Changanacherry, Kerala",
               "Addr2":null,
               "Loc":"Changanacherry",
               "Pin":parseInt("686104"),
               "Stcd":"32",
               "Ph":null,
               "Em":null
			},    

			"BuyerDtls":{
               "Gstin":this.buyerGSTIN,
               "LglNm":this.buyerLegalName,
               "TrdNm":null,
               "Pos":this.buyerState+'',
               "Addr1":this.buyerAddress,
               "Addr2":null,
               "Loc":this.buyerLocation,
               "Pin":parseInt(this.buyerPIN),
               "Stcd":this.buyerState+'',
               "Ph":null,
               "Em":null
			},

			"ShipDtls":null,

            "ValDtls":this.ValDtls,

			"EwbDtls":{
               "TransId":null,
               "TransName":null,
               "TransMode":"1",
               "Distance":0,
               "TransDocNo":"0",
               "TransDocDt":"01/04/2021",
               "VehNo":"XXX",
               "VehType":"R"
			},

			"ItemList": tempItemsList
            }];

			if(this.enableConsignee) {
				jsonData[0]["ShipDtls"] = {
						"Gstin":this.consigneeGSTIN,
						"LglNm":this.consigneeLegalName,
						"TrdNm":null,
						"Addr1":this.consigneeAddress,
						"Addr2":null,
						"Loc":this.consigneeLocation,
						"Pin":parseInt(this.consigneePIN),
						"Stcd":this.consigneeState+''
					};
            }
            
            if(this.enableEwayBill) {
                jsonData[0]["EwbDtls"]['Distance'] = parseInt(this.ewayBillDistance);
                jsonData[0]["EwbDtls"]['TransDocNo'] = this.ewayBillTransportDocNo;
                jsonData[0]["EwbDtls"]['TransDocDt'] = moment(this.ewayBillTransportDocDate).format("DD/MM/YYYY");
                jsonData[0]["EwbDtls"]['VehNo'] = this.ewayBillVehicleNo;
            } else {
                jsonData[0]["EwbDtls"] = null;
            }

			const data = JSON.stringify(jsonData)
			const blob = new Blob([data], {type: 'text/plain'})
			const e = document.createEvent('MouseEvents'),
			a = document.createElement('a');
			a.download = "test.json";
			a.href = window.URL.createObjectURL(blob);
			a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
			e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
			a.dispatchEvent(e);
		},
		addItem() {
			let tempObj = [{
				"SlNo": (this.itemsList.length+1)+'',
				"PrdDesc":"M F Kraft Paper",
				"IsServc":"N",
				"HsnCd":"48041100",
				"Qty":0,
				"Unit":"KGS",
				"UnitPrice":0,
				"TotAmt":0,
				"Discount":0,
				"PreTaxVal":0,
				"AssAmt":0,
				"GstRt":12,
				"IgstAmt":0,
				"CgstAmt":0,
				"SgstAmt":0,
				"CesRt":0,
				"CesAmt":0,
				"CesNonAdvlAmt":0,
				"StateCesRt":0,
				"StateCesAmt":0,
				"StateCesNonAdvlAmt":0,
				"OthChrg":0,
				"TotItemVal":0
			}];
			this.itemsList.push(...tempObj);
		}
	}
}
</script>

<style>
	.smallText {
		font-size: 9pt;
		font-weight: bold;
	}
</style>