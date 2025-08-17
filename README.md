# Sip_Design_Prototype

    using System;
    using System.Collections.Generic;
    using System.IO;
    using UnityEngine;
    
    // This script is used to check the time in Unity
    
    public class checkTime : MonoBehaviour
    {
    
    
        class FileHandler // Renamed to avoid conflict with System.IO.File
        {
    
    
            public static bool FileExists(string path)
            {
                // Check if the file exists in the specified path
                return System.IO.File.Exists(path);
            }
    
            public static void CreateFile()
            {
                string directoryPath = "C:/Users/Mason/Documents/UnityGames/Sip Prototype/Assets/Resources/";
                string fileName = "Time.txt";
                string fullPath = Path.Combine(directoryPath, fileName);
    
                // Ensure the directory exists
                if (!Directory.Exists(directoryPath))
                {
                    Directory.CreateDirectory(directoryPath);
                }
    
                // Create the file
                System.IO.File.WriteAllText(fullPath, "");
            }
            public static void WriteTime(string time)
            {
                // Write the current time to the file
                System.IO.File.WriteAllText(path, time);
                Debug.Log("Time written to file: " + time);
            }
    
            public static void CheckFile()
            {
                if (!FileExists(path))
                {
                    // If the file does not exist, create it
                    Debug.Log("File does not exist, creating file.");
                    CreateFile();
                }
                else
                {
                    // If the file exists, log a message
                    Debug.Log("File exists.");
    
                }
            }
            public static string ReadTime(string path)
            {
                // Read the time from the file
                string TimeRead = System.IO.File.ReadAllText(path);
                Debug.Log("Time read from file: " + TimeRead);
                return TimeRead;
            }
    
            public static List<int> CalcTime(string path)
            {
                // Calculate the time difference from the last recorded time
                string TimeRead = ReadTime(path); // Read the time from the file
    
                DateTime dateCurrent = DateTime.Now;
                DateTime PrevDate = DateTime.Parse(TimeRead); // Read the previous time from the file
                int years = (dateCurrent - PrevDate).Days / 365;
                int months = (dateCurrent - PrevDate).Days / 30;
                int days = (dateCurrent - PrevDate).Days;
                int hours = (dateCurrent - PrevDate).Hours;
                int minutes = (dateCurrent - PrevDate).Minutes;
                int seconds = (dateCurrent - PrevDate).Seconds;
                string DispTime = $"Years: {years}, Months: {months}, Days: {days}, Hours: {hours} Minutes: {minutes}, Seconds: {seconds}";
    
                Debug.Log("Time difference calculated: " + DispTime);
                List<int> EachTime = new List<int>();
                EachTime.Add(years);
                EachTime.Add(months);
                EachTime.Add(days);
                EachTime.Add(hours);
                EachTime.Add(minutes);
                EachTime.Add(seconds);
                return EachTime;
    
    
            }
    
            public static string ThresholdCalc(int Threshold_years, int Threshold_months, int Threshold_days, int Threshold_hours, int Threshold_minutes, int Threshold_seconds, List<int> EachTime)
            {
                if (Threshold_years < 0 || Threshold_months < 0 || Threshold_days < 0 || Threshold_hours < 0 || Threshold_minutes < 0 || Threshold_seconds < 0)
                {
                    Debug.LogError("Threshold values cannot be negative.");
                    return "Invalid threshold values.";
                }
                else if (EachTime[0] > Threshold_years ||
                    EachTime[1] > Threshold_months ||
                    EachTime[2] > Threshold_days ||
                    EachTime[3] > Threshold_hours ||
                    EachTime[4] > Threshold_minutes ||
                    EachTime[5] > Threshold_seconds)
                {
                    Debug.Log("Threshold exceeded.");
                    return "Threshold exceeded.";
                }
                else
                {
                    Debug.Log("Threshold not exceeded.");
                    return "Threshold not exceeded.";
    
                }
    
            }
        }
    
        public int Threshold_years;
        public int Threshold_months;
        public int Threshold_days;
        public int Threshold_hours;
        public int Threshold_minutes;
        public int Threshold_seconds;
    
        public static string path = "C:/Users/Mason/Documents/UnityGames/Sip Prototype/Assets/Resources/Time.txt";
        void Awake()
        {
    
            // This method is called when the script instance is being loaded
            Debug.Log("checkTime script is loaded.");
            FileHandler.CheckFile();
            FileHandler.ThresholdCalc(Threshold_years, Threshold_months, Threshold_days, Threshold_hours, Threshold_minutes, Threshold_seconds, FileHandler.CalcTime(path));
    
    
    
        }
        private void OnApplicationQuit()
        {
            FileHandler.WriteTime(System.DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss")); // Write the current time to the file
            Debug.Log("Application is quitting, time written to file.");
        }
    
        void Start()
        {
        }
    
        // Update is called once per frame
        void Update()
        {
        }
    }

