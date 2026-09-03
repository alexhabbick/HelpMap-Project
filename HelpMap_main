import streamlit as st
import folium
from streamlit_folium import st_folium
import requests
import json


st.title("Help Map Project")

st.write("Hello! This website is designed to help people in need by providing information " \
        "about organizations and resources available in the Toronto area. The goal of this project "
        "is to make it easier for people who are experiencing homelessness, newcomers, refugees, " \
        "immigrants, or financial difficulties to find the support they need.")

st.write("Finding basic necessities and important services can be difficult, especially for someone " \
        "who may not know where to look or what resources are available. This website brings different types " \
        "of resources together in one place, including homeless shelters, free health clinics, clothing services, " \
        "food programs, and other community organizations.")

st.write("The purpose of this project is to make these resources easier to find and understand. By providing " \
        "information such as the location, services offered, and contact information for different organizations, " \
        "people can more easily find a place that may be able to help them.")
st.write("")

filter_values = st.pills(
    "Filters For Map",
    ["Food", "Health Clinic", "Shelters", "Clothes"],
    selection_mode="multi", default=["Food", "Health Clinic", "Shelters", "Clothes"]
)
m = folium.Map(location=[43.642481, -79.387099], zoom_start = 12)

file = open("HelpMap_File.txt", "r")
lines = file.readlines()
for line in lines:
    name, category, latitude, longitude, address = line.strip().split(",")
    if "Food" in filter_values:
        if category == "Food":
            folium.Marker(location = [float(latitude), float(longitude)], tooltip=name, popup=address, icon=folium.Icon(color="green", icon="info-sign")).add_to(m)
    if "Health Clinic" in filter_values:
        if category == "Health Clinic":
            folium.Marker(location = [float(latitude), float(longitude)], tooltip=name, popup=address, icon=folium.Icon(color="red", icon="info-sign")).add_to(m)
    if "Shelters" in filter_values:
        if category == "Shelter":
            folium.Marker(location = [float(latitude), float(longitude)], tooltip=name, popup=address, icon=folium.Icon(color="blue", icon="info-sign")).add_to(m)
    if "Clothes" in filter_values:
        if category == "Clothes":
            folium.Marker(location = [float(latitude), float(longitude)], tooltip=name, popup=address, icon=folium.Icon(color="orange", icon="info-sign")).add_to(m)
file.close()
st_folium(m, width = 700, height = 500)

st.write("Add a location that can help people that are in need:")
add_name_location = st.text_input("What is the name of the location?")
add_location_address = st.text_input("Address of Location:")

st.write(add_name_location)
add_category_location = st.radio("What is the type of the Location:",["Food","Health Clinic","Shelter","Clothes"])
st.write(add_location_address)
add_location_address = add_location_address + " Toronto"


add_button = st.button("Add")

if add_button:
    if add_location_address != "" and add_name_location != "":
        add_name_location = add_name_location.replace(",", "")
        add_location_address = add_location_address.replace(",", "")
        add_location_address = add_location_address + " Toronto"

        url = "https://nominatim.openstreetmap.org/search"
        param = {
            "q": add_location_address,
            "format": "json",
            "limit": 1
        }
        response = requests.get(
            url,
            params=param,
            headers={"User-Agent": "HelpMapProject/1.0"}
        )
        data = json.loads(response.text)
        if len(data) > 0:
            add_latitude = data[0]["lat"]
            add_longitude = data[0]["lon"]

            add_location_address = add_location_address.replace(" Toronto", "")

            file = open("HelpMap_File.txt", "a")
            file.write(
                add_name_location + "," + add_category_location + "," + add_latitude + "," + add_longitude + "," + add_location_address + "\n")
            file.close()

            st.success("Location added!")
        else:
            st.error("Could not find that address.")
    else:
        st.error("Please enter a name and address.")

st.subheader("Other sources that can help:")
st.link_button("211Ontario", "https://211ontario.ca/search/")
st.link_button("City of Toronto - Housing and Shelter", "https://www.toronto.ca/community-people/housing-shelter/?")
st.link_button("211Central", "https://211central.ca/")
