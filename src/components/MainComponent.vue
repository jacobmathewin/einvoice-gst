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
				<input type="text" class="form-control text-center font-weight-bold" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Legal Name
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Address
			</div>
			<div class="col-4">
				<textarea name="" id="" cols="30" rows="3" class="form-control font-weight-bold"></textarea>
			</div>
		</div>
		

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee Location
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee PIN
			</div>
			<div class="col-2">
				<input type="text" class="form-control font-weight-bold" />	
			</div>
		</div>

		<div class="row pt-1" v-show="enableConsignee">
			<div class="col-6 text-right pt-2">
				Consignee State
			</div>
			<div class="col-4">
				<input type="text" class="form-control font-weight-bold" />	
			</div>
		</div>
		
		<div class="row text-primary mt-3">
			<div class="col-6 text-right">Item Details</div>
			<div class="col-4">
				<button class="btn btn-outline-primary btn-sm" v-on:click="addItem()">Add Item</button>
			</div>
		</div>

		<div class="row text-center">
			<div class="col-1">S.No.</div>
			<div class="col-1">Description</div>
			<div class="col-1">HSN Code</div>
			<div class="col-1">Qty (kg)</div>
			<div class="col-1">Unit Price</div>
			<div class="col-2">Value</div>
			<div class="col-1">CGST</div>
			<div class="col-1">SGST</div>
			<div class="col-1">IGST</div>
			<div class="col-2">Total</div>
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
			invoiceNo: '252',
			invoiceDate: new Date(),
			buyerGSTIN: '32AADCF6541R1ZA',
			buyerLegalName: 'FLEXTRUS CORRUGATED PACKAGING BOXES INDIA PVT LTD',
			buyerAddress: 'V/872-A, PERUMPILLIL BUILDING, RAMAMANGALAM ROAD, KOLENCHERY. P.O, ERNAKULAM',
			buyerLocation: 'Kolenchery',
			buyerPIN: 686111,
			buyerState: '32',
			buyerStateName: 'Kerala',
			buyerStateSuggestions: '',
			enableConsignee: false,
			itemsList: [],
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

		getFormattedDate(date) {
			return moment(date).format("DD/MM/YYYY");
		},

		saveFile: function() {

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
               "Pos":this.buyerState,
               "Addr1":this.buyerAddress,
               "Addr2":null,
               "Loc":this.buyerLocation,
               "Pin":parseInt(this.buyerPIN),
               "Stcd":this.buyerState,
               "Ph":null,
               "Em":null
			},

			"ShipDtls":null,

			"ValDtls":{
               "AssVal":28222.50,
               "IgstVal":0,
               "CgstVal":1693.35,
               "SgstVal":1693.35,
               "CesVal":0,
               "StCesVal":0,
               "Discount":0,
               "OthChrg":0,
               "RndOffAmt":0,
               "TotInvVal":31609.20
			},

			"EwbDtls":{
               "TransId":null,
               "TransName":null,
               "TransMode":"1",
               "Distance":50,
               "TransDocNo":"44",
               "TransDocDt":"12/04/2021",
               "VehNo":"KL11N1901",
               "VehType":"R"
			},

			"ItemList": this.itemsList
            }];

			if(this.enableConsignee) {
				jsonData[0]["ShipDtls"] = {
						"Gstin":"32AALFB0400A1ZL",
						"LglNm":"BISMI PACK",
						"TrdNm":null,
						"Addr1":"Vattakkerimuttath Kadavil Road, Aroor PO, Alappuzha",
						"Addr2":null,
						"Loc":"Aroor",
						"Pin":688534,
						"Stcd":"32"
					};
			}

			console.log(jsonData);

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
				"SlNo":"1",
				"PrdDesc":"M F Kraft Paper",
				"IsServc":"N",
				"HsnCd":"48041100",
				"Qty":795,
				"Unit":"KGS",
				"UnitPrice":35.50,
				"TotAmt":28222.5,
				"Discount":0,
				"PreTaxVal":0,
				"AssAmt":28222.5,
				"GstRt":12,
				"IgstAmt":0,
				"CgstAmt":1693.35,
				"SgstAmt":1693.35,
				"CesRt":0,
				"CesAmt":0,
				"CesNonAdvlAmt":0,
				"StateCesRt":0,
				"StateCesAmt":0,
				"StateCesNonAdvlAmt":0,
				"OthChrg":0,
				"TotItemVal":31609.20
			}];
			this.itemsList.push(...tempObj);
			console.log(this.itemsList);
		}
	}
}
</script>