# Snapshot Data Definitions
Each day a new snapshot file csv is created and uploaded to this repo based on the observed holdings and defi positions for addresses on integrated chains.  Stride produces this data in conjuction with Numia through a mix of chain state snapshots and queries.  The snapshots are taken at slightly different random times each day and only represent the instantaneous value at that moment.  All integrations are captured within a 5 minute window (so the same funds cannot be moved across integrations and double counted by accident).

Each csv row represents one balance for a particular integration type uniquely defined by the combination of chain, denom, address, application-id.  Denoms and therefore also the amounts are always represented in the microunit, so for example `denom=stuatom` and `amount=2603942` would mean 2.6 stAtom because the micro denom of uatom means 1e6; similarly the micro unit adydx means 1e18.

The address_on_chain field is the address of the owner of these funds on the chain itself. Because only some chain address types can be automatically converted to other chain addresses, the address_on_stride field is only present for certain integrations.  When present, the address_on_stride is a useful tool to track the same owner's holdings across different chains and applications.

The application-id field uniquely defines what specific integration this row is tracking.  There are the more general application and category fields, but these are mostly useful for grouping similar types of integrations at a high level. When `category=balance` and `application=bank`, this simply means funds are being held in an address on that particular chain.  For example, if `chain=neutron`, `denom=stadydx`, `application-id=neutron-bank` this row represents someone holding stDYDX on neutron chain in liquid form -- not locked into any defi integration.  Note that certain bank applications are actually built on top of other chains, so `application-id=demex-bank` is actually on the `chain=carbon`


### Defi Application Types
The most common types of defi integrations are liquidity being provided to a DEX, and positions in a lending protocol.  When there is a DEX liquidity position, there are always two different denoms paired together but we might only track whichever one is relevant to our rewards tracking usecase.  Sometimes non-staked tokens are tracked as part of liquidity because our rewards program would incentivize both sides of the pool -- for example when `application-id=osmosis-cl-stTIA-TIA-1428` there will be rows for both `denom=utia` and `denom=stutia` because both sides counted towards our rewards program.  If you only care about the stTokens make sure to correctly filter rows based on the denom column.

There are different types of liquidity positions.  So 'xyk' represents a traditional balanced pool type for a multiplicative market maker -- if we track both sides of the pool they are likely to be of roughly equal value (but because of denom differences, the amounts can differ). On the other hand 'pcl' or 'cl' pools are concentrated liquidity positions where one side can be weighted more heavily than the other and there is no guarantee that the amounts a single address has provided are similar on each side. 

Lending positions can be either collateral positions or borrowing positions.  Collateral means that this address has given the protocol this amount and denom, borrowing means the protocol has given this address the amount instead.  If you are trying to reward holders of a specific token type, you should avoid those rows where the application-id contains lending-borrowed and depending on your usecase you may even want to subtract this amount.  Note that in the case of mars lending protocol, there are different versions on both neutron and on osmosis chains.


### Tracked Integrations
These are all the integrations tracked at any time by the application-id and for which denoms.  However, not all of them were active at the same time so below are when each integration was started or stopped being tracked by snapshot files.  You can use this to determine when to look for specific data in the snapshot files.

- Bank Holdings:
    - **stride-bank**: (utia, stutia, stadym, stuatom, stuosmo, stadydx, stucmdx, stusomm, stuluna, stujuno, stuumee, stustars, stinj, staevmos)
    - **osmosis-bank**: (utia, stutia, stadym, stuatom, stuosmo, stadydx, stucmdx, stusomm, stuluna, stujuno, stuumee, stustars, stinj, staevmos)
    - **neutron-bank**: (utia, stutia, stadym, stuatom, stuosmo, stadydx)
    - **agoric-bank**: (stutia, stuatom, stuosmo)
    - **demex-bank**: (utia, stutia)
    - **dymension-bank**: (utia, stutia, stadym)
    - **manta-bank**: (utia)

- Liquidity:
    - **astroport-pcl-DYDX-stDYDX-78ds0v**: (adydx, stadydx)
    - **astroport-pcl-TIA-stTIA-wslqc6**: (utia, stutia)
    - **astroport-pcl-stATOM-ATOM-4uga62**: (stuatom)
    - **astroport-pcl-DYM-stDYM-u30aw5**: (stadym)    
    - **osmosis-xyk-ATOM-stATOM-803**: (stuatom)
    - **osmosis-xyk-stOSMO-OSMO-833**: (stuosmo)    
    - **osmosis-cl-stATOM-ATOM-1136**: (uatom, stuatom)
    - **osmosis-cl-stOSMO-OSMO-1252**: (uosmo, stuosmo)    
    - **osmosis-cl-stATOM-ATOM-1283**: (stuatom)
    - **osmosis-cl-stDYDX-DYDX-1423**: (adydx, stadydx)
    - **osmosis-cl-stTIA-TIA-1428**: (utia, stutia)
	- **osmosis-cl-stDYM-DYM-1566**: (stadym)
	- **dymension-xyk-DYM-TIA-3**: (utia)
	- **dymension-xyk-DYM-stTIA-6**: (stutia)
	- **dymension-xyk-DYM-stDYM-11**: (stadym)

- Lending:
	- **agoric-lending-collateral**: (stutia, stuatom, stuosmo)
	- **demex-lending-borrowed**: (utia, stutia)
	- **demex-lending-collateral**: (utia, stutia)
	- (neutron) **mars-lending-collateral**: (stuatom)
	- (osmosis) **mars-lending-borrowed**: (utia)
	- (osmosis) **mars-lending-collateral**: (utia, stutia, stuatom, stuosmo)
	- **umee-lending-borrowed**: (utia, stutia, stuatom)
	- **umee-lending-collateral**: (utia, stutia, stuatom)



### Integrations Tracked by Date

#### 2024-02-01
- Started tracking holdings: 
    - **stride-bank**: (stutia, stuatom, stuosmo, stadydx, stusomm, stuluna, stujuno, stucmdx, stuumee, stustars, stinj, staevmos)    


#### 2024-02-05
- Started tracking holdings:
	- **neutron-bank**: (stutia, stuatom, stuosmo, stadydx)
	- **osmosis-bank**: (stutia, stuatom, stuosmo, stadydx, stucmdx, stusomm, stinj, stuluna, stuumee, stujuno, stustars, staevmos)
- Started tracking Liquidity:
    - **astroport-pcl-DYDX-stDYDX-78ds0v**: (adydx, stadydx)
    - **astroport-pcl-TIA-stTIA-wslqc6**: (utia, stutia)
    - **osmosis-cl-stATOM-ATOM-1136**: (uatom, stuatom)
    - **osmosis-cl-stDYDX-DYDX-1423**: (adydx, stadydx)
    - **osmosis-cl-stOSMO-OSMO-1252**: (uosmo, stuosmo)    
    - **osmosis-cl-stTIA-TIA-1428**: (utia, stutia)


#### 2024-02-28 
- Started tracking holdings:
	- **agoric-bank**: (stutia)
	- **demex-bank** (carbon chain): (utia, stutia)
	- **dymension-bank**: (utia, stutia)
	- **manta-bank**: (utia)
	- **stride-bank**: (utia)
	- **osmosis-bank**: (utia)
	- **neutron-bank**: (utia)
- Started tracking Liquidity:
	- **dymension-xyk-DYM-TIA-3**: (utia)
	- **dymension-xyk-DYM-stTIA-6**: (stutia)
- Started tracking Lending:
	- **agoric-lending-collateral**: (stutia)
	- **demex-lending-borrowed**: (utia, stutia)
	- **demex-lending-collateral**: (utia, stutia)
	
- Stopped tracking holdings:
	- **stride-bank**: (stusomm, stuluna, stadydx, stujuno, stucmdx, stuumee, stustars, stinj, staevmos)
	- **neutron-bank**: (stadydx)
	- **osmosis-bank**: (stucmdx, stusomm, stinj, stuluna, stuumee, stujuno, stadydx, stustars, staevmos)		  
- Stopped tracking Liquidity:
	- **astroport-pcl-DYDX-stDYDX-78ds0v**: (adydx, stadydx)
	- **osmosis-cl-stATOM-ATOM-1136**: (uatom)
	- **osmosis-cl-stDYDX-DYDX-1423**: (adydx, stadydx)
	- **osmosis-cl-stOSMO-OSMO-1252**: (uosmo)
					
- Continued tracking holdings: 
	- **stride-bank**: (stutia, stuatom, stuosmo) 
    - **neutron-bank**: (stutia, stuatom, stuosmo)
	- **osmosis-bank**: (stutia, stuatom, stuosmo)
- Continued tracking Liquidity:	
	- **osmosis-cl-stATOM-ATOM-1136**: (stuatom)
	- **osmosis-cl-stOSMO-OSMO-1252**: (stuosmo)
	

#### 2024-03-01
- Started tracking holdings:
	- **agoric-bank**: (stuatom, stuosmo)
- Started tracking Liquidity:		
    - **astroport-pcl-stATOM-ATOM-4uga62**: (stuatom)
    - **osmosis-cl-stATOM-ATOM-1283**: (stuatom)
    - **osmosis-xyk-ATOM-stATOM-803**: (stuatom)
    - **osmosis-xyk-stOSMO-OSMO-833**: (stuosmo)
- Started tracking Lending:
	- **agoric-lending-collateral**: (stuatom, stuosmo)
		
	    		    
#### 2024-03-14
- Started tracking holdings:
	- **stride-bank**: (stadym)
	- **dymension-bank**: (stadym)
- Started tracking Liquidity:	
	- **dymension-xyk-DYM-stDYM-11**: (stadym)
	- **osmosis-cl-stDYM-DYM-1566**: (stadym)


#### 2024-03-16
- Started tracking holdings:
	- **osmosis-bank**: (stadym)


#### 2024-03-18
- Started tracking holdings:
	- **neutron-bank**: (stadym)
- Started tracking Liquidity:    
	- **astroport-pcl-DYM-stDYM-u30aw5**: (stadym)


#### 2024-04-16
- Started tracking Lending:
	- (neutron) **mars-lending-collateral**: (stuatom)
	- (osmosis) **mars-lending-borrowed**: (utia)
	- (osmosis) **mars-lending-collateral**: (utia, stutia, stuatom, stuosmo)
	- **umee-lending-borrowed**: (utia, stutia, stuatom)
	- **umee-lending-collateral**: (utia, stutia, stuatom)
