# Changelog

An overview of all changes to the NtS xsd is provided in this file.

## [5.0] - July 2021

### Changed
- Changed the namespace to https://ris.cesni.eu/_assets/NtS_XSD/5.0.5.0
- Journal removed from xsd file


## [4.6] - April 2021

### Changed
- CR 198 - Improvement of withdrawal process for FTM limitations and messages
		Add notice withdrawn flag, to indicate that the entire message is withdrawn.
          <xs:element name="notice_withdrawn" type="xs:boolean" minOccurs="0">
            <xs:annotation>
              <xs:documentation>Notice Withdrawn flag set to "1" when the message is Withdrawn. The subject code of previous version must remain the same.</xs:documentation>
            </xs:annotation>
          </xs:element>
     	Add withdrawn_time, to indicate a limitation period is withdrawn
          <xs:element name="withdrawn_time" type="xs:dateTime" minOccurs="0">
            <xs:annotation>
              <xs:documentation>Date and time of withdrawal including time zone</xs:documentation>
            </xs:annotation>
          </xs:element>
		subject_code_enum
			de-activate <xs:enumeration value="CANCEL"/>
- CR 199 - Amendment of format of geo-coordinates in NtS messages  
 	    <xs:complexType name="coordinate_type">
		    <xs:sequence>
			    <xs:element name="lat">
				    <xs:simpleType>
					    <xs:restriction base="xs:float"> 
					    </xs:restriction>
				    </xs:simpleType>
			    </xs:element>
			    <xs:element name="long">
				    <xs:simpleType>
					    <xs:restriction base="xs:float">
					    </xs:restriction>
				    </xs:simpleType>
			    </xs:element>
		    </xs:sequence>
	    </xs:complexType>   
- CR 201 -  Support for multiple permissible dimension combinations for vessels/convoys in FTM 
		Change of value float to nts:value_type
          <xs:element name="value" type="nts:value_type" minOccurs="0" maxOccurs="4">
            <xs:annotation>
              <xs:documentation>Value of limitation (i.e.  max draught)</xs:documentation>
            </xs:annotation>
          </xs:element>
          <xs:complexType name="value_type">
            <xs:annotation>
              <xs:documentation>Value of the limitation, with optional type to indicate the size of a vessel/convoy</xs:documentation>
            </xs:annotation>
            <xs:simpleContent>
              <xs:extension base="xs:float">
                <xs:attribute name="dimension_type" type="nts:dimension_type_code_enum" use="optional" />
              </xs:extension>
            </xs:simpleContent>
          </xs:complexType>
          <xs:simpleType name="dimension_type_code_enum">
            <xs:restriction base="xs:string">
              <xs:maxLength value="3"/>
              <xs:enumeration value="LEN"/>
              <xs:enumeration value="BRE"/>
              <xs:enumeration value="HEI"/>
              <xs:enumeration value="DRA"/>
            </xs:restriction>
          </xs:simpleType>
		Add <xs:enumeration value="PERDIM" /> in limitation_code_enum  
- Changed the namespace to https://ris.cesni.eu/_assets/NtS_XSD/4.0.4.6


## [4.0.4.1-Hotfix] - Feb 2021

### Fixed
- Correction of regex in ISRS-code restrictions. The locode can contain alphanumeric characters.
      <xs:simpleType name="isrs_code_type">
      <xs:annotation>
        <xs:documentation>ISRS location code, unique identification of the geo object as defined in RIS Index encoding guide</xs:documentation>
      </xs:annotation>
      <xs:restriction base="xs:string">
        <xs:length value="20"/>
        <xs:pattern value="[A-Z]{2}[A-Z2-9]{3}[A-Z0-9]{5}[A-Z0-9]{5}[0-9]{5}" />
      </xs:restriction>
      </xs:simpleType>
- Correction of inconsistancy in XSD - change maxLength of limitation_code_enum from 6 to 10
        <xs:simpleType name="limitation_code_enum">
            <xs:restriction base="xs:string">
              <xs:maxLength value="10"/>

## [4.5] - Oct 2020

### Changed
- CR 195 - Introduce network_part and object to WRM to comply with the structure of other NtS-messages
	Changed object in WRM to optional & add network part
      <xs:element name="network_part" type="nts:geo_network_type" minOccurs="0" maxOccurs="unbounded">
        <xs:annotation>
          <xs:documentation>A part on the network delimited by two points</xs:documentation>
        </xs:annotation>
      </xs:element>
      <xs:element name="object" type="nts:geo_object_type" minOccurs="0" maxOccurs="unbounded">
        <xs:annotation>
          <xs:documentation>Object section</xs:documentation>
        </xs:annotation>
      </xs:element>
- CR 196: Streamlining of codes - removal of unused enumerations  
		reason_code_enum
			de-activate <xs:enumeration value="OTHER"/>
		interval_code_enum
			de-activate <xs:enumeration value="EXC"/>
		Removal of following enumerations:
			reason_code_enum
					deleted value "INFSER"
			subject_code_enum
				deleted values "OBSTRU", "PAROBS", "DELAY", "VESLEN", "VESHEI", "VESBRE", "VESDRA", "AVALEN", "CLEHEI", "CLEWID", "AVADEP", "NOMOOR", "SERVIC",
					"NOSERV", "SPEED", "WAVWAS", "PASSIN", "ANCHOR", "OVRTAK", "MINPWR", "DREDGE", "WORK", "EVENT", "CHGMAR", "CHGSER", "SPCMAR", "EXERC", "LEADEP",
					"LEVDEC", "LEVRIS", "LIMITA", "MISECH", "ECDISU", "NEWOBJ", "CHWWY", "CONWWY", "DIVER", "SPECTR", "LOCRUL", "VHFCOV", "HIGVOL", "TURNIN",
					"CONBRE", "CONLEN", "REMOBJ"  
- CR 197 - Self Describing Message Part 2
		Change <xs:element name="originator"> to <xs:element name="publisher">
		Remove <xs:element name="source" minOccurs="0"> in FTM
		Add <xs:element name="source" minOccurs="0"> in Identification
		Make coordinate mandatory in location_type
		  <xs:element name="coordinate" type="nts:coordinate_type">
			<xs:annotation>
			  <xs:documentation>coordinate</xs:documentation>
			</xs:annotation>
		  </xs:element>
		Rename preticted to forecast in measure_type
			<xs:element name="forecast" type="xs:boolean">
				<xs:annotation>
					<xs:documentation>Forecast (1 or true) or real measurement (0 or false)</xs:documentation>
				</xs:annotation>
			</xs:element>
		Make measuredate mandatory in weather_report
		  <xs:element name="measuredate" type="xs:dateTime">
			<xs:annotation>
			  <xs:documentation>Date and time of measurement or forecast value including timezone</xs:documentation>
			</xs:annotation>
		  </xs:element>
		Make interval_code mandatory in limitation_period
    		<xs:element name="interval_code" type="nts:interval_code_enum" default="CON">
				<xs:annotation>
					<xs:documentation>Interval for limitation if applicable</xs:documentation>
				</xs:annotation>
			</xs:element>

## [4.4] - June 2020

### Added    
- CR 193 - Add Geographic Impact of an NtS message
      Add optional element geographic_impact to network_part and object
      The following coordinate system must be used: WGS84 latitude/longitude (EPSG:4326)
      This geographic data is NOT  for navigational purposes
        <xs:element name="geographic_impact" minOccurs="0">
          <xs:annotation>
            <xs:documentation>Geographical impact</xs:documentation>
          </xs:annotation>
          <xs:simpleType>
            <xs:restriction base="xs:string">
            </xs:restriction>
          </xs:simpleType>
        </xs:element>


## [4.3] - June 2020

### Fixed
- Correction of inconsistancy in XSD - change maxLength of limitation_code_enum from 6 to 10
    	<xs:simpleType name="limitation_code_enum">
		    <xs:restriction base="xs:string">
			  <xs:maxLength value="10"/>

### Changed   
- CR 192 - Self Describing NtS Message
      Add <xs:complexType name="location_type"> with folowing elements
        - isrs-code (instead of id)
        - type_code
        - un_locode
        - fairway_section_code
        - object_reference_code
        - fairway_hectometre
        - coordinate
      Add localisation_name_type: object_name is mandatory, but un_location_name is optional cause dismar/berth without transshipment don't always have a un-location
        <xs:complexType name="localisation_name_type">
          <xs:sequence>
            <xs:element name="un_location_name" type ="nts:name_type" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="object_name" type ="nts:name_type" minOccurs="1" maxOccurs="unbounded"/>
          </xs:sequence>
        </xs:complexType>
      Add object_location_type: for an object it is mandatory to fill in the location_type and the localisation_name, so one knows the name of the object
        <xs:complexType name="object_location_type">
          <xs:sequence>
            <xs:element name="location" type ="nts:location_type" maxOccurs="1" />
            <xs:element name="localisation_name" type ="nts:localisation_name_type" maxOccurs="1"/>
          </xs:sequence>
        </xs:complexType>
      Add network_point_location_type: a network_part is always delimited by two ISRS locations. network_point_location_type contains ONE of these two points.
      This element contains the mandatory location_type and the optional localisation_name_type, so it's not mandatory to give an object_name and un_location_name for a dismar
          <xs:complexType name="network_point_location_type">
            <xs:sequence>
              <xs:element name="location" type ="nts:location_type" maxOccurs="1"/>
              <xs:element name="localisation_name" type ="nts:localisation_name_type" minOccurs="0" maxOccurs="1"/>
            </xs:sequence>
          </xs:complexType>
      Update geo_object_type:
        - Use <xs:element name="geo_location" type="nts:object_location_type">
        - fairway_name: mandatory element instead of optional <xs:element name="fairway_name" type="nts:name_type" minOccurs="1" maxOccurs="unbounded">
        - add route_name: optional element <xs:element name="route_name" type="nts:name_type" minOccurs="0" maxOccurs="unbounded">
      Add geo_network_type instead of fairwaysection type
          - Add <xs:element name="geo_location_from" type="nts:network_point_location_type">
          - Add <xs:element name="geo_location_to" type="nts:network_point_location_type">
          - fairway_name: mandatory element instead of optional
          - Add <xs:element name="route_name" type="nts:name_type" minOccurs="0" maxOccurs="unbounded">
          - Add <xs:element name="type_code" type="nts:type_code_enum">, to define if the network part is a fairway, lake, canal, river
      FTM
        Use network_part instead of fairway_section <xs:element name="network_part" type="nts:geo_network_type" minOccurs="0" maxOccurs="unbounded">
      ICEM
        Use network_part instead of fairway_section <xs:element name="network_part" type="nts:geo_network_type">
      WERM
        Use network_part inline with FTM and ICEM message <xs:element name="network_part" type="nts:geo_network_type">

## [4.2] - Apr 2018

### Changed          
- CR 187 Make weather_class_code optional in the Weather Related Message (WERM)
			<xs:element name="weather_class_code" type="nts:weather_class_code_enum" minOccurs="0" maxOccurs="unbounded">
- CR 190 NtS XSD Limitation Grouping
			FTM
			delete 
				<xs:choice maxOccurs="unbounded">
						<xs:element name="fairway_section" type="nts:fairway_section_type">
        		<xs:element name="object" type="nts:object_type">
			delete 
				<xs:complexType name="fairway_section_type">
					<xs:complexType name="object_type">
			instead
				  <xs:complexType name="ftm_limitation_group_type">
					<xs:sequence>
					  <xs:choice maxOccurs="unbounded">
						<xs:element name="fairway_section" type="nts:geo_object_type" minOccurs="0" maxOccurs="unbounded">
						  <xs:annotation>
							<xs:documentation>Fairway section</xs:documentation>
						  </xs:annotation>
						</xs:element>
						<xs:element name="object" type="nts:geo_object_type" minOccurs="0" maxOccurs="unbounded">
						  <xs:annotation>
							<xs:documentation>Object section</xs:documentation>
						  </xs:annotation>
						</xs:element>
					  </xs:choice>
					  <xs:element name="limitation_group" type="nts:limitation_group_type" minOccurs="0" maxOccurs="unbounded">
						<xs:annotation>
						  <xs:documentation>Group of limitations and periods for Fairways and Objects</xs:documentation>
						</xs:annotation>
					  </xs:element>
					</xs:sequence>
				  </xs:complexType>
			ICEM
			Fairway section no longer contains the limitation section
				<xs:element name="fairway_section" type="nts:geo_object_type">
			WERM
			delete <xs:element name="fairway_section" type="nts:fairway_section_werm_type">
			instead add <xs:element name="fairway_section" type="nts:geo_object_type">	
			WRM
			Change geo_object into object to be inline with other messagetypes
				<xs:element name="object" type="nts:geo_object_type">

## [4.0] - Nov 2016

### Added
- CR 157 New NTS type code for bunkering/fuelling station
			add value BNS to <xs:simpleType name="type_code_enum"> 

### Fixed
- Typo in type_code_enum:
      <xs:enumeration value="LBK"/>, changed to  <xs:enumeration value="LKB"/>


### Changed
- Textual changes in XSD description: harmonize documentation of elements with NtS encoding guides and 'XML_Scheme_V 4 0 4 0.xslx'

- CR 155 Changes in Weather_item_code, Weather_category_code and Reason_code to improve information on weather conditions
			add values to <xs:simpleType name="weather_category_code_enum">: values 0->22
			add value WERMCO to <xs:simpleType name="reason_code_enum">
- CR 176 Make the “nts_number” in WRM and WERM optional
			<xs:element name="nts_number" type="nts:nts_number_type" minOccurs="0">
        
- CR 177 Make the “position_code” optional in the limitation section
			<xs:element name="position_code" type="nts:position_code_enum" minOccurs="0">

## [3.8] - Aug 2014 

### Added
- CR 153 Add target_group_code and direction_code to the limitation section
- Add simple type "isrs_code_type" in XSD
        <xs:simpleType name="isrs_code_type">
          <xs:annotation>
            <xs:documentation>ISRS location code, unique identification of the geo object as defined in RIS Index encoding guide</xs:documentation>
          </xs:annotation>
          <xs:restriction base="xs:string">
          <xs:length value="20"/>
          <xs:pattern value="[A-Z]{2}[A-Z]{3}[A-Z0-9]{5}[A-Z0-9]{5}[0-9]{5}" />
          </xs:restriction>
        </xs:simpleType>
- Update "geo_object_type": changed the definition of element "id" from type "xs:string" to "nts:isrs_code_type".

## [3.7] - Jun 2014 

### Fixed
- Corrected typo frm to ftm

### Changed
- CR 148 update limitation_code_enum:
			add <xs:enumeration value="NOBERT"/>, 'no berthing'
- Changed documentation for element subject_code
     <xs:documentation>Subject code must contain one of the following: Announcement (ANNOUN), Warning (WARNIN), Notice withdrawn (CANCEL) or Information service (INFSER). More information on the use of codes can be found in the NtS Encoding Guide.</xs:documentation>

## [3.6] - May 2014
NtS Task Force XML Structure Enhancement 
          
### Changed
- CR 141 Make ‘validity_period’ section mandatory in the WRM and ICEM
			update "wrm_type" and "icem_type"
				<xs:element name="validity_period" type="nts:validity_period_type"/>
- CR 142 Make 'date_end' optional in 'validity_period'
			update "validity_period_type"
				<xs:element name="date_end" type="xs:date" minOccurs="0">
					<xs:annotation>
						<xs:documentation>End date of the validity period</xs:documentation>
					</xs:annotation>
				</xs:element>
- CR 143 Introduction of WRM (predicted) value confidence interval
			update "measure_type"
			add fields "value_min" and "value_max"
				<xs:element name="value_min" type="xs:float" minOccurs="0">
					<xs:annotation>
						<xs:documentation>Lowest value of confidence interval</xs:documentation>
					</xs:annotation>
				</xs:element>
				<xs:element name="value_max" type="xs:float" minOccurs="0">
					<xs:annotation>
						<xs:documentation>Highest value of confidence interval</xs:documentation>
					</xs:annotation>
				</xs:element>
- CR 144 Correction of geo-coordinate field length
			update "coordinate_type"
				LAT:     [d]d_mm.mmm[m]_N =>  minlength=10, maxlength=12
				LONG: [d][d]d_mm.mmm[m]_E =>  minlength=10, maxlength=13
- CR 146 Addition of new Interval_code ‘WRD’ for “Monday to Friday except holidays”
			update "interval_code_enum"
				add WRD
- CR 149 Change the mandatory fairway_section part to optional for object-related FTM
			update "ftm_type"
				<xs:choice maxOccurs="unbounded">
			<xs:element name="fairway_section" type="nts:fairway_section_type" maxOccurs="unbounded">
				<xs:annotation>
					<xs:documentation>Fairway section</xs:documentation>
				</xs:annotation>
			</xs:element>
			<xs:element name="object" type="nts:object_type" minOccurs="0" maxOccurs="unbounded"/>
				</xs:choice>
			update "geo_object_type"
				add
			<xs:element name="fairway_name" minOccurs="0">
				<xs:annotation>
					<xs:documentation>Waterway name</xs:documentation>
				</xs:annotation>
				<xs:simpleType>
					<xs:restriction base="xs:string">
						<xs:maxLength value="256"/>
					</xs:restriction>
				</xs:simpleType>
			</xs:element>			
- CR 151 Remove measure_code “NOM”
			update "measure_code_enum"
				obsolete values due to 151 but still valid for backwards compatibility: "NOM"
				
## [3.5] - February 2014
NtS Task Force XML Structure Enhancement 

### Changed  
- CR 128 update reason_code_enum
			obsolete values due to CR128 but still valid for backwards compatibility: "INFSER"
		 update subject_code_enum
			obsolete values due to CR128 but still valid for backwards compatibility:
            "OBSTRU", "PAROBS", "DELAY", "VESLEN", "VESHEI", "VESBRE", "VESDRA", "AVALEN", "CLEHEI", "CLEWID", "AVADEP", "NOMOOR", "SERVIC",
            "NOSERV", "SPEED", "WAVWAS", "PASSIN", "ANCHOR", "OVRTAK", "MINPWR", "DREDGE", "WORK", "EVENT", "CHGMAR", "CHGSER", "SPCMAR", "EXERC", "LEADEP",
            "LEVDEC", "LEVRIS", "LIMITA", "MISECH", "ECDISU", "NEWOBJ", "CHWWY", "CONWWY", "DIVER", "SPECTR", "LOCRUL", "VHFCOV", "HIGVOL", "TURNIN",
            "CONBRE", "CONLEN", "REMOBJ"
- CR 130 update geo_object_type, use the ISRS code for the unique identification of the geo object in the element "id" in geo_object_type:
			<xs:element name="id" maxOccurs="2">
				<xs:annotation>
					<xs:documentation>Unique identification of the geo object, country specific</xs:documentation>
				</xs:annotation>
				<xs:simpleType>
					<xs:restriction base="xs:string">
						<xs:length value="20"/>
						<xs:pattern value="[A-Z]{2}[A-Z]{3}[A-Z0-9]{5}[A-Z0-9]{5}[0-9]{5}" />
					</xs:restriction>
				</xs:simpleType>
			</xs:element>
- CR 131 update type_code_enum:
			add <xs:enumeration value="LBK"/>
			add <xs:enumeration value="BRO"/>
- CR 132 update geo_object_type, optional position information added to the geo_object_type:
		  <xs:element name="position_code" type="nts:position_code_enum" minOccurs="0">
			<xs:annotation>
			  <xs:documentation>Position of the object, e.g. left, right, north, big, small, old, new</xs:documentation>
			</xs:annotation>
		  </xs:element>
- CR 135 Provision of the unit of values within NtS messages
		  add <xs:simpleType name="unit_enum">
		  update limitation_type, optional unit information added
			<xs:element name="unit" type="nts:unit_enum" minOccurs="0"/>
		  update measure_type, optional unit information added
			<xs:element name="unit" type="nts:unit_enum" minOccurs="0"/>
		  update weather_item_type, optional unit information added
			<xs:element name="unit" type="nts:unit_enum" minOccurs="0"/>
- CR 136 Use of xs:dateTime type in the NtS XSD
		  update identification_type, 'date_issue' and 'time_issue' merged in one element 'date_issue' and changed from optional to mandatory 
				<xs:element name="date_issue" type="xs:dateTime">
					<xs:annotation>
						<xs:documentation>Date and Time of editing including timezone</xs:documentation>
					</xs:annotation>
				</xs:element>
		  update measure_type, 'measuredate' and 'measuretime' merged in one element 'measuredate'
				<xs:element name="measuredate" type="xs:dateTime">
					<xs:annotation>
						<xs:documentation>Date and time of measurement</xs:documentation>
					</xs:annotation>
				</xs:element>
		  update ice_condition_type, 'measuredate' and 'measuretime' merged in one element 'measuredate'
				<xs:element name="measuredate" type="xs:dateTime">
					<xs:annotation>
						<xs:documentation>Date and time of measurement</xs:documentation>
					</xs:annotation>
				</xs:element>
		  update weather_report_type, 'measuredate' and 'measuretime' merged in one element 'measuredate'
			<xs:element name="measuredate" type="xs:dateTime" minOccurs="0">
			  <xs:annotation>
				<xs:documentation>Date and time of measurement</xs:documentation>
			  </xs:annotation>
			</xs:element>
- CR 140 direction_code_min_enum and direction_code_max_enum are merged to weather_direction_code_enum
			<xs:simpleType name="weather_direction_code_enum">
				<xs:restriction base="xs:string">
					<xs:maxLength value="3"/>
					<xs:enumeration value="N"/>
					<xs:enumeration value="NE"/>
					<xs:enumeration value="E"/>
					<xs:enumeration value="SE"/>
					<xs:enumeration value="S"/>
					<xs:enumeration value="SW"/>
					<xs:enumeration value="W"/>
					<xs:enumeration value="NW"/>
					<xs:enumeration value="WRB"/>
				</xs:restriction>
			</xs:simpleType>

## [3.4] - November 2013
NtS Task Force XML Structure Enhancement 

### Added
- CR 087	add <xs:simpleType name="weather_class_code_enum">
			add <xs:simpleType name="weather_item_code_enum">
			add <xs:simpleType name="weather_category_code_enum">
			add <xs:simpleType name="direction_code_min_enum">
			add <xs:simpleType name="direction_code_max_enum">
- CR 127	update type_code_enum:
			add <xs:enumeration value="RES"/>

### Changed
- CR 087	update weather_class_code, changed type from string to weather_class_code_enum
			update weather_item_code, changed type from string to weather_item_code_enum
			update weather_category_code, changed type from string to weather_category_code_enum
			update direction_code_min, changed type from string to direction_code_min_enum
			update direction_code_max, changed type from string to direction_code_max_enum
- CR 110	update direction_code_max_enum:
				changed xs:maxLength from 2 to 3
				add <xs:enumeration value="WRB"/>
- CR 128	update communication_code_enum, add the value "EI":
				<xs:enumeration value="EI"/>
			update reason_code_enum
				add values "CHGMAR", "DAMMAR", "FALMAT", "MISECH", "HEARIS", "HIGVOL", "ECDISU", "LOCRUL", "NEWOBJ", "OBUNWA", "VHFCOV", "REMOBJ", "LEVRIS", "SPCMAR"
				deleted value "INFSER"
			update limitation_code_enum
				add value "LEADEP"
			update subject_code_enum
				add value "INFSER"
			deleted values "OBSTRU", "PAROBS", "DELAY", "VESLEN", "VESHEI", "VESBRE", "VESDRA", "AVALEN", "CLEHEI", "CLEWID", "AVADEP", "NOMOOR", "SERVIC",
				"NOSERV", "SPEED", "WAVWAS", "PASSIN", "ANCHOR", "OVRTAK", "MINPWR", "DREDGE", "WORK", "EVENT", "CHGMAR", "CHGSER", "SPCMAR", "EXERC", "LEADEP",
				"LEVDEC", "LEVRIS", "LIMITA", "MISECH", "ECDISU", "NEWOBJ", "CHWWY", "CONWWY", "DIVER", "SPECTR", "LOCRUL", "VHFCOV", "HIGVOL", "TURNIN",
				"CONBRE", "CONLEN", "REMOBJ"
- CR 129	update geo_object_type, increase the length of the element “name” for the geo object from 64 to 256 characters:
				<xs:element name="name">
					<xs:annotation>
						<xs:documentation>Name of the geo object (local)</xs:documentation>
					</xs:annotation>
					<xs:simpleType>
						<xs:restriction base="xs:string">
							<xs:maxLength value="256"/>
						</xs:restriction>
					</xs:simpleType>
				</xs:element>

## [3.3] - June 2013
NtS Task Force XML Structure Enhancement 

### Added
- CR 102 update target_group_code_enum with the value 'WOC' for workside craft:
		    add <xs:enumeration value="WOC"/>
- CR 104 update reason_code_enum with the value 'ICE' for ice:
		    add <xs:enumeration value="ICE"/>
- CR 105 update target_group_code_enum with the value 'MOV' for motorized vessels and 'NMV' for non motorized vessels:
		    add <xs:enumeration value="MOV"/>
			add <xs:enumeration value="NMV"/>
- CR 106 update reason_code_enum with the value 'OBSTAC' for obstacle:
		    add <xs:enumeration value="OBSTAC"/>
- CR 111 update reference_code_enum with the value 'HBO' for High Water Level before overflow Danube Commission:
		    add <xs:enumeration value="HBO"/>
			
## [3.2] - November 2012
NtS Task Force XML Structure Enhancement 

### Changed
- CR 120 Change namespace to http://www.ris.eu/nts/MajorStandard.MinorStandard.MajorXSD.MinorXSD to be compliant with the namespace definition for the NtS Webservices.
        Example: The version of the NtS standard is 3.0 and for the XSD 3.2 gives the namespace: http://www.ris.eu/nts/3.0.3.2

## [3.1] - December 2011
NtS Task Force XML Structure Enhancement 

### Added
- CR 084 add new <xs:element name="label" minOccurs="0"> and new <xs:element name="remark" minOccurs="0"> into communication_type section
- CR 086 add new <xs:complexType name="nts_number_type"> in each message type FTM/ICEM/WERM/WRM.
            <xs:complexType name="nts_number_type"> consists of <xs:element name="organisation">, <xs:element name="year">, <xs:element name="number"> and <xs:element name="serial_number">
            delete <xs:element name="year">, <xs:element name="number"> and <xs:element name="serial_number"> in <xs:complexType name="ftm_type">
            update <xs:element name="year"> min. value 2000 is changed to 1900
            update <xs:element name="number"> max. value 9999 is increased to 99999999
            add new <xs:element name="internal_id" type="nts:internal_id_type" minOccurs="0" /> in each section (Identification, FTM, ICEM, WERN and WRM) 

### Fixed
- CR 074 The element <validity_period.date_end> is a mandatory element of type <xs:date> as already foreseen in the XSD version 3.0. The indefinite End date validity period should be "9999-12-31" instead of "99999999" mentioned in the Standard (see XML Message Specification)

### Changed
- CR 080 update communication_code_enum. Harmonisation of communication codes with ERI standard:
            <xs:enumeration value="TEL"/> changed into <xs:enumeration value="TE"/>
            <xs:enumeration value="VHF"/> changed into <xs:enumeration value="AP"/>
            <xs:enumeration value="INT"/> changed into <xs:enumeration value="AH"/>
            <xs:enumeration value="TXT"/> changed into <xs:enumeration value="TT"/>
            <xs:enumeration value="FAX"/> changed into <xs:enumeration value="FX"/>
            <xs:enumeration value="LIG"/> changed into <xs:enumeration value="LS"/>
            <xs:enumeration value="FLA"/> changed into <xs:enumeration value="FS"/>
            <xs:enumeration value="SOU"/> changed into <xs:enumeration value="SO"/>
		
- CR 088 namespace changed to http://www.ris.eu/nts/3.1
- CR 089 update <xs:simpleType name="country_code_enum">, 
            add <xs:enumeration value="RS"/> and <xs:enumeration value="ME"/>
            delete <xs:enumeration value="CS"/>
- CR 090 update <xs:simpleType name="position_code_enum">, 
            add <xs:enumeration value="RY"/> and <xs:enumeration value="GY"/>
- CR 093 add <xs:complexType name="difference_type"> in replacement of <xs:element name="difference" type="xs:float" minOccurs="0"> into the element <xs:complexType name="measure_type">
- CR 097 add <xs:element name="measuredate" type="xs:date"> and <xs:element name="measuretime" type="xs:time">into <xs:complexType name="weather_report_type">

## [3.0] - June 2011
NtS Task Force Web Services

### Changed
- CR 088 namespace changed to http://www.ris.eu/nts/3.0

## [3.0] - November 2008
NTS Expert Group 

### Added
- CR 043 add <xs:enumeration value="NOSHORE"/> into enumeration limitation_code_enum
- CR 046 add <xs:enumeration value="CONBRE"/> into enumeration limitation_code_enum
- CR 047 add <xs:enumeration value="CONLEN"/> into enumeration limitation_code_enum
- CR 053 add <xs:enumeration value="LO"/> into enumeration regime_code_enum
- CR 056 add <xs:enumeration value="CONBRE"/> into enumeration subject_code_enum
- CR 057 add <xs:enumeration value="CONLEN"/> into enumeration subject_code_enum
- CR 059 add <xs:enumeration value="REMOBJ"/> into enumeration subject_code_enum
- CR 060 add <xs:enumeration value="SMA"/> into enumeration target_group_code_enum
- CR 061 add <xs:enumeration value="CND"/> into enumeration target_group_code_enum
- CR 062 add <xs:enumeration value="DUK"/> into enumeration type_code_enum
- CR 067 add <xs:enumeration value="VTC"/> into enumeration type_code_enum
- CR 070 add <xs:enumeration value="STRIKE"/>, <xs:enumeration value="FLOMAT"/>, <xs:enumeration value="EXPLOS"/> into enumeration reason_code_enum

## [2.8.2] - April-May 2008
Structural changes to the schema, preserving the content model
Addition of three optional attributes in RIS_Message_Type for paging mechanism

## [2.8.1] - December 2007
NTS Expert Group 

### Added
- CR 022  add <xs:enumeration value="NOM"/> to enumeration of the measure_code = no measurement
- CR 029  add <xs:enumeration value="SLI"/> to the enumeration of the type_code = ship lift
- CR 030  add <xs:enumeration value="TURNIN"/> to the enumeration of the limitation_code and subject_code = no turning
- CR 031  add <xs:enumeration value="LOA"/> to the enumeration of the target_group = loaded vessel -->

### Changed
- CR 003  <xs:element name="name"> instead of <xs:element name="name" maxOccurs="2">
- CR 011  use of <xs:date>   <xs:time> for date / time elements
- CR 015  <xs:element name="value" type="xs:float" minOccurs="0"/> instead of <xs:element name="value" type="xs:float"/>

## [2.8] - May 2007
Weather related message section added:

### Added
- <werm> section.
- Choice option for <ftm>, <wrm>, <icem>, <werm> sections.

### Changed
- <ftm>, <wrm>, <icem>, <werm> sections to mandatory (choice)
- <RIS_Message\Identification> => <identification> (corrected tag to lowercase)

## [2.7] - September 2006
Initial release
