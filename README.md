layout.js
import { Stack } from "expo-router";

export default function RootLayout() {
  return (
   
    <Stack>
      <Stack.Screen name="index" options={{ title: 'Api App' }} />
      <Stack.Screen name="contact" options={{ title: 'Contact App Access' }} />
      <Stack.Screen name="location" options={{ title: 'Location App Access' }} />
      <Stack.Screen name="battery" options={{ title: 'Battery App Access' }} />
    </Stack>
    
  );
}

battery.js
import { useEffect, useState } from "react";
import { Text, View, StyleSheet } from "react-native";
import * as Battery from "expo-battery";

export default function BatteryPage() {
  const [level, setLevel] = useState(null);

  useEffect(() => {
    Battery.getBatteryLevelAsync().then(val => setLevel(val * 100));
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.text}>Battery Level: {level}%</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center" },
  text: { fontSize: 24 }
});


contact.js
import { useEffect, useState } from "react";
import { Text, View, StyleSheet, FlatList } from "react-native";
import * as Contacts from "expo-contacts";

export default function ContactsPage() {
  const [contactList, setContactList] = useState([]);

  useEffect(() => {
    (async () => {
      const { status } = await Contacts.requestPermissionsAsync();
      if (status === "granted") {
        const { data } = await Contacts.getContactsAsync({
          fields: [Contacts.Fields.PhoneNumbers]
        });
        setContactList(data);
      }
    })();
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Contacts</Text>
      <FlatList
        data={contactList}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <Text style={styles.item}>{item.name}</Text>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20 },
  title: { fontSize: 24, marginBottom: 10 },
  item: { fontSize: 18, borderBottomWidth: 1, paddingVertical: 5 }
});



index.js
import { Link } from "expo-router";

export default function Home() {
    return (
        <View style={StyleSheet.container}>  
            <Link href="/contact">Go to Contact App Access</Link>
            <Link href="/location">Go to Location App Access</Link>
            <Link href="/battery">Go to Battery App Access</Link>
        </View>
    )
}
const styles = StyleSheet.create({
    container: {flex:1,padding:20}
});
import {View, StyleSheet} from "react-native"





location.js
import { useEffect, useState } from "react";
import { Text, View, StyleSheet } from "react-native";
import * as Location from "expo-location";

export default function LocationPage() {
  const [coords, setCoords] = useState(null);

  useEffect(() => {
    (async () => {
      const { status } = await Location.requestForegroundPermissionsAsync();
      if (status === "granted") {
        const loc = await Location.getCurrentPositionAsync({});
        setCoords(loc.coords);
      }
    })();
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Your Location</Text>

      {coords ? (
        <>
          <Text>Latitude: {coords.latitude}</Text>
          <Text>Longitude: {coords.longitude}</Text>
        </>
      ) : (
        <Text>Loading location…</Text>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center" },
  title: { fontSize: 22, marginBottom: 10 }
});
