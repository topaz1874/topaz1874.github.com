
####audit tags


 	<node id="757860928" visible="true" version="2" changeset="5288876" timestamp="2010-07-22T16:16:51Z" user="uboot" uid="26299" lat="41.9747374" lon="-87.6920102">
  		<tag k="amenity?" v="fast_food"/>
  		<tag k="cuisine" v="sausage"/>
  		<tag k="NAME" v="Shelly's Tasty Freeze"/>
 	</node>
 
 ----

	def audit():
		for event, elem in ET.iterparse(osm_file, events=("start",)):
			if elem.tag == "way":
				for tag in elem.iter("tag"):
					if is_street_name(tag):
						audit_street_type(street_types, tag.attrib['v'])
						
						
						
---

####auditing street names

	expected = ["Street", "Avenue", "Boulevard"]
	
	street_types = defaultdict(set)
	
	street_type_re = re.compile(r'\b\S+\.?$', re.IGNORECASE)
		
	def is_street_name(elem):
		return (elem.attrib['k'] == "addr:street")
		
	def audit_street_type(street_types, street_name):
		m = street_type_re.search(street_name)
		if m :
			street_type = m.group()
			if street_type not in expected:
					street_types[street_type].add(street_name)
				
				
