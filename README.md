# trafzad
-- 
-- Output of registry data into file:
-- DSRC_R36_Source.ASN
-- in format need to operate on the ASN source files.
-- 
-- Run on Mini-Edit Version 3.1.500 
-- From file: \DSRC_36\Dsrc_rev036.ITS
-- Last Changed:  [Mod: 10/28/2009 3:08:03 PM]
-- This export was created on 11/11/2009  at 1:15:00 PM 
-- web: http://www.itsware.net/itsschemas/DSRC/DSRC-03-00-36/
-- ###################################################
-- 
-- 
-- Run this file with a line like:
-- asn1 source.txt -errorfile errs.txt -noun
-- 
-- The local module consisting of DEs / DFs and MSGs
DSRC DEFINITIONS AUTOMATIC TAGS::= BEGIN 
 
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- Start of entries from table Dialogs...
-- This table typicaly contains dialog and operational exchange entries.
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
 
-- Table contains no entries.  
 
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- Start of entries from table Messages...
-- This table typicaly contains message entries.
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
 
 
-- MSG_A_la_Carte  (ACM) (Desc Name) Record 1
AlaCarte ::=  SEQUENCE {
   msgID         DSRCmsgID,                 
                 -- the message type
   data          AllInclusive,  
                 -- any possible set of data items here  
   crc           MsgCRC OPTIONAL,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_BasicSafetyMessage  (BSM) (Desc Name) Record 2
BasicSafetyMessage ::=  SEQUENCE {
   -- Part I
   msgID       DSRCmsgID,                 -- 1 byte
   
   -- Sent as a single octet blob 
   blob1       BSMblob, 

   --   
   -- The blob consists of the following 38 packed bytes: 
   -- 
   -- msgCnt      MsgCount,             -x- 1 byte
   -- id          TemporaryID,          -x- 4 bytes
   -- secMark     DSecond,              -x- 2 bytes

   -- pos      PositionLocal3D,
     -- lat       Latitude,             -x- 4 bytes
     -- long      Longitude,            -x- 4 bytes
     -- elev      Elevation,            -x- 2 bytes
     -- accuracy  PositionalAccuracy,   -x- 4 bytes

   -- motion   Motion,
     -- speed     TransmissionAndSpeed, -x- 2 bytes
     -- heading   Heading,              -x- 2 byte
     -- angle     SteeringWheelAngle    -x- 1 bytes
     -- accelSet  AccelerationSet4Way,  -x- 7 bytes 
   
   -- control  Control,
   -- brakes      BrakeSystemStatus,    -x- 2 bytes
   
   -- basic    VehicleBasic,
   -- size        VehicleSize,          -x- 3 bytes
   
   -- Part II, sent as required 
   -- Part II, 
   safetyExt     VehicleSafetyExtension OPTIONAL, 
   status        VehicleStatus          OPTIONAL,  

   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_BasicSafetyMessage_Verbose (Desc Name) Record 3
BasicSafetyMessageVerbose ::=  SEQUENCE {
   -- Part I, sent at all times 
   msgID       DSRCmsgID,            -- App ID value, 1 byte
   
   msgCnt      MsgCount,             -- 1 byte
   id          TemporaryID,          -- 4 bytes
   secMark     DSecond,              -- 2 bytes
   -- pos      PositionLocal3D,
   lat         Latitude,             -- 4 bytes 
   long        Longitude,            -- 4 bytes
   elev        Elevation,            -- 2 bytes
   accuracy    PositionalAccuracy,   -- 4 bytes
   
   -- motion   Motion,
   speed       TransmissionAndSpeed, -- 2 bytes
   heading     Heading,              -- 2 bytes
   angle       SteeringWheelAngle,   -- 1 bytes
   accelSet    AccelerationSet4Way,  -- 7 bytes
   
   -- control  Control,
   brakes      BrakeSystemStatus,    -- 2 bytes
   
   -- basic    VehicleBasic,
   size        VehicleSize,          -- 3 bytes
   
   -- Part II, sent as required 
   -- Part II, 
   safetyExt   VehicleSafetyExtension OPTIONAL, 
   status      VehicleStatus          OPTIONAL,  
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_CommonSafetyRequest (CSR) (Desc Name) Record 4
CommonSafetyRequest ::=  SEQUENCE {
   msgID       DSRCmsgID,                 
   msgCnt      MsgCount OPTIONAL, 
   id          TemporaryID OPTIONAL,               
                                          
   -- Note: Uses the same request as probe management
   requests    SEQUENCE (SIZE(1..32)) OF RequestedItem,

   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_EmergencyVehicleAlert (EVA) (Desc Name) Record 5
EmergencyVehicleAlert ::=  SEQUENCE {
   msgID           DSRCmsgID,   
   id              TemporaryID OPTIONAL, 
   rsaMsg          RoadSideAlert, 
				-- the DSRCmsgID inside this 
                                -- data frame is set as per the 
                                -- RoadSideAlert.  The CRC is
                                -- set to a value of zero. 
   responseType    ResponseType                   OPTIONAL, 
   details         EmergencyDetails               OPTIONAL,   
                   -- Combines these 3 items: 
                   -- SirenInUse,                     
                   -- LightbarInUse,                  
                   -- MultiVehicleReponse,

   mass            VehicleMass                    OPTIONAL,
   basicType       VehicleType                    OPTIONAL,  
                                -- gross size and axle cnt
   
   -- type of vehicle and agency when known
   vehicleType     ITIS.VehicleGroupAffected      OPTIONAL,      
   responseEquip   ITIS.IncidentResponseEquipment OPTIONAL, 
   responderType   ITIS.ResponderGroupAffected    OPTIONAL,    
   crc             MsgCRC,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_IntersectionCollisionAvoidance  (ICA) (Desc Name) Record 6
IntersectionCollision ::=  SEQUENCE {
   msgID          DSRCmsgID,                 
   msgCnt         MsgCount,                 
   id             TemporaryID,               
   secMark        DSecond OPTIONAL,
   path           PathHistory, 
                  -- a set of recent path histories
   intersetionID  IntersectionID,      
                  -- the applicable Intersection, from the MAP-GID
                  -- the best applicable movement, from the MAP-GID
   laneNumber     LaneNumber,          
                  -- the best applicable Lane, from the MAP-SPAT-GID
                  -- zero sent if unknown
   eventFlag      EventFlags,
                  -- used to convey vehicle Panic Events,  
                  -- Set to indicate "Intersection Violation" 
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_MapData  (MAP) (Desc Name) Record 7
MapData ::=  SEQUENCE {
   msgID           DSRCmsgID, 
   msgCnt          MsgCount, 
   name            DescriptiveName OPTIONAL,
   layerType       LayerType  OPTIONAL,
   layerID         LayerID  OPTIONAL,
   intersections   SEQUENCE (SIZE(1..32)) OF 
                   Intersection OPTIONAL, 

   -- other objects may be added at this layer, tbd,
   -- this might become a nested CHOICE statement 
   -- roadSegments    SEQUENCE (SIZE(1..32)) OF 
   --                 RoadSegments OPTIONAL, 
   -- curveSegments   SEQUENCE (SIZE(1..32)) OF 
   --                 curveSegments OPTIONAL,    
   
   -- wanted: some type of data frame describing how 
   -- the data was determined/processed to go here
   dataParameters   DataParameters OPTIONAL, 
   crc              MsgCRC,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_NMEA_Corrections  (NMEA) (Desc Name) Record 8
NMEA-Corrections ::=  SEQUENCE {
   msgID     DSRCmsgID,   
   rev       NMEA-Revision,
             -- the specific edition of the standard
             -- that is being sent, normally 2.0
   msg       NMEA-MsgType,
             -- the message and sub-message type, as
             -- defined in the revision being used
   -- NOTE as the message type is also in the payload, 
   wdCount   INTEGER (0..1023),
             -- a count of bytes to follow
   payload   NMEA-Payload, 
   ...
   }
 
 
 
-- MSG_ProbeDataManagement  (PDM) (Desc Name) Record 9
ProbeDataManagement ::=  SEQUENCE {
   msgID                DSRCmsgID,        -- This is a unique message 
                                          -- identifier, NOT related to 
                                          -- the PSID\PSC
   sample               Sample,           -- identifies vehicle  
                                          -- population affected
   directions           HeadingSlice,       
                                          -- Applicable headings/directions
   term CHOICE {                          
      termtime          TermTime,         -- Terminate management process 
                                          -- based on Time-to-Live
      termDistance      TermDistance      -- Terminate management process 
                                          -- based on Distance-to-Live
      },
   snapshot CHOICE {
      snapshotTime      SnapshotTime,     -- Collect snapshots based on time
      snapshotDistance  SnapshotDistance  -- Collect snapshots based on Distance
      },
   txInterval           TxTime,           -- Time Interval at which to send snapshots
   cntTthreshold        Count,            -- number of thresholds that will be changed
   dataElements SEQUENCE (SIZE(1..32)) OF 
                        VehicleStatusRequest,  
                                          -- a data frame and its assoc thresholds
   
   ...
   }
 
 
 
-- MSG_ProbeVehicleData  (PVD) (Desc Name) Record 10
ProbeVehicleData ::=  SEQUENCE {
   msgID           DSRCmsgID,            -- App ID value, 1 byte
   segNum          ProbeSegmentNumber OPTIONAL,   
                                         -- a short term Ident value
                                         -- not used when ident is used
   probeID         VehicleIdent OPTIONAL, 
                                         -- ident data for selected 
                                         -- types of vehicles    
   startVector     FullPositionVector,   -- the space and time of 
                                         -- transmission to the RSU
   vehicleType     VehicleType,          -- type of vehicle, 1 byte
   cntSnapshots    Count OPTIONAL,   
                                         -- a count of how many snaphots 
                                         -- type entries will follow
   snapshots       SEQUENCE (SIZE(1..32)) OF Snapshot,               
                                         -- a seq of name-value pairs 
                                         -- along with the space and time 
                                         -- of the first measurement set
   ... -- # LOCAL_CONTENT 
   } -- Est size about 64 bytes plus snapshot sizes (about 12 per)
 
 
 
-- MSG_RoadSideAlert  (RSA) (Desc Name) Record 11
RoadSideAlert ::=  SEQUENCE { 
   msgID         DSRCmsgID,                 
                 -- the message type. 
   msgCnt        MsgCount,   
   typeEvent     ITIS.ITIScodes,
                 -- a category and an item from that category 
                 -- all ITS stds use the same types here
                 -- to explain the type of  the 
                 -- alert / danger / hazard involved
                 -- two bytes in length
   description   SEQUENCE (SIZE(1..8)) OF ITIS.ITIScodes OPTIONAL,
                 -- up to eight ITIS code entries to further
                 -- describe the event, give advice, or any 
                 -- other ITIS codes
                 -- up to 16 bytes in length 
   priority      Priority OPTIONAL,  
                 -- the urgency of this message, a relative
                 -- degree of merit compared with other 
                 -- similar messages for this type (not other
                 -- message being sent by the device), nor a 
                 -- priority of display urgency
                 -- one byte in length
   heading       HeadingSlice  OPTIONAL,       
                 -- Applicable headings/direction
   extent        Extent OPTIONAL,  
                 -- the spatial distance over which this
                 -- message applies and should be presented 
                 -- to the driver
                 -- one byte in length
   positon       FullPositionVector OPTIONAL, 
                 -- a compact summary of the position,
                 -- heading, rate of speed, etc of the 
                 -- event in question. Including stationary
                 -- and wide area events. 
   furtherInfoID FurtherInfoID OPTIONAL,
                 -- a link to any other incident 
                 -- information data that may be available 
                 -- in the normal ATIS incident description 
                 -- or other messages
                 -- 1~2 bytes in length
    crc          MsgCRC
    }
 
 
 
-- MSG_RTCM_Corrections  (RTCM) (Desc Name) Record 12
RTCM-Corrections ::=  SEQUENCE {
   msgID     DSRCmsgID, 
   msgCnt    MsgCount, 
   rev       RTCM-Revision,
             -- the specific edition of the standard
             -- that is being sent

   anchorPoint FullPositionVector OPTIONAL,
   -- precise observer position, if needed

   -- precise ant position and noise data
   rtcmHeader  RTCMHeader,
   -- octets of:
   -- status      GPSstatus               
   -- antOffsets  AntennaOffsetSet(x,y,z)                      
   
   -- one or more RTCM messages
   rtcmSets    SEQUENCE (SIZE(1..5)) OF RTCMmsg,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_SignalRequestMessage  (SRM) (Desc Name) Record 13
SignalRequestMsg ::=  SEQUENCE {
   msgID           DSRCmsgID, 
   msgCnt          MsgCount,            

   -- Request Data 
   request        SignalRequest,
                  -- the specific request to the intersection
                  -- contains IntersectionID, cancel flags,
                  -- requested action, optional lanes data
   
   timeOfService  DTime  OPTIONAL,
                  -- the time in the near future when service is
                  -- requested to start
   
   endOfService   DTime  OPTIONAL,
                  -- the time in the near future when service is
                  -- requested to end

   transitStatus  TransitStatus OPTIONAL, 
                  -- additional information pertaining 
                  -- to transit events

   -- User Data
   vehicleVIN     VehicleIdent  OPTIONAL,
                  -- a set of unique strings to identify the requesting vehicle
   
   vehicleData    BSMblob,
                  -- current position data about the vehicle
   
   status         VehicleRequestStatus  OPTIONAL,
                  -- current status data about the vehicle
 
   ...
   }
 
 
 
-- MSG_SignalStatusMessage  (SSM) (Desc Name) Record 14
SignalStatusMessage ::=  SEQUENCE { 
   msgID         DSRCmsgID,   
   msgCnt        MsgCount, 
   id            IntersectionID,  
                 -- this provides a unique mapping to the  
                 -- intersection map in question 
                 -- which provides complete location  
                 -- and approach/move/lane data 
                 -- as well as zones for priority/preemption
   status        IntersectionStatusObject, 
                 -- general status of the signal controller
   priority      SEQUENCE (SIZE(1..7)) OF SignalState OPTIONAL, 
                 -- all active priority state data 
                 -- is found here 
   priorityCause VehicleIdent  OPTIONAL, 
                 -- vehicle that requested  
                 -- the current priority   
   prempt        SEQUENCE (SIZE(1..7)) OF SignalState OPTIONAL, 
                 -- all active preemption state data 
                 -- is found here 
   preemptCause  VehicleIdent  OPTIONAL, 
                 -- vehicle that requested  
                 -- the current preempt 
   transitStatus TransitStatus OPTIONAL, 
                 -- additional information pertaining 
                 -- to transit event, if that is the active event
   ...
   }
 
 
 
-- MSG_SignalPhaseAndTiming Message  (SPAT) (Desc Name) Record 15
SPAT ::=  SEQUENCE {
   msgID         DSRCmsgID, 
   name          DescriptiveName OPTIONAL, 
                 -- human readable name for this collection 
                 -- to be used only in debug mode

   intersections SEQUENCE (SIZE(1..32)) OF IntersectionState,
                 -- sets of SPAT data (one per intersection)  

   ... -- # LOCAL_CONTENT
   }
 
 
 
-- MSG_TravelerInformation Message  (TIM) (Desc Name) Record 16
TravelerInformation ::=  SEQUENCE {
   msgID           DSRCmsgID, 
   packetID        UniqueMSGID    OPTIONAL,    
   urlB            URL-Base       OPTIONAL,  
   dataFrameCount  Count          OPTIONAL, 
   
   dataFrames SEQUENCE (SIZE(1..8)) OF SEQUENCE {
   
      -- Part I, Frame header
      frameType   TravelerInfoType,  -- (enum, advisory or road sign)
      msgId       CHOICE  {
                     furtherInfoID   FurtherInfoID,
                                     -- links to ATIS msg
                     roadSignID      RoadSignID  
                                     -- an ID to other data
                     },
      startYear   DYear OPTIONAL, 
                  -- Current year used if missing
      startTime   MinuteOfTheYear,
      duratonTime MinutesDuration,
      priority    SignPrority,
  
      -- Part II, Applicable Regions of Use
     commonAnchor         Position3D     OPTIONAL, 
                          -- a shared anchorpoint 
     commonLaneWidth      LaneWidth      OPTIONAL,   
                          -- a shared lane width 
     commonDirectionality DirectionOfUse OPTIONAL, 
                          -- a shared direction of use
     regions  SEQUENCE (SIZE(1..16)) OF ValidRegion,
   
      -- Part III, Content
      content  CHOICE  {
                  advisory       ITIS.ITIScodesAndText,           
                                 -- typical ITIS warnings
                  workZone       WorkZone,       
                                 -- work zone signs and directions
                  genericSign    GenericSignage, 
                                 -- MUTCD signs and directions
                  speedLimit     SpeedLimit,     
                                 -- speed limits and cautions
                  exitService    ExitService     
                                 -- roadside avaiable services
                  -- other types may be added in future revisions
                  },  --# UNTAGGED 
      url     URL-Short OPTIONAL  -- May link to image or other content
      },
   crc        MsgCRC,
   ... -- # LOCAL_CONTENT
   }
 
 
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- Start of entries from table Data_Frames...
-- This table typicaly contains data frame entries.
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
 
 
-- DF_AccelerationSet4Way (Desc Name) Record 1
AccelerationSet4Way ::= OCTET STRING (SIZE(7)) 
   -- composed of the following:
   -- SEQUENCE {
   --    long Acceleration,          -x- Along the Vehicle Longitudinal axis
   --    lat  Acceleration,          -x- Along the Vehicle Lateral axis
   --    vert VerticalAcceleration,  -x- Along the Vehicle Vertical axis
   --    yaw  YawRate
   --    }
 
 
 
-- DF_AccelSteerYawRateConfidence (Desc Name) Record 2
AccelSteerYawRateConfidence ::=  SEQUENCE {
   yawRate             YawRateConfidence, 
                       -- 3 bits
   acceleration        AccelerationConfidence, 
                       -- 3 bits
   steeringWheelAngle  SteeringWheelAngleConfidence 
                       -- 2 bits
   }
 
 
 
-- DF_AllInclusive (Desc Name) Record 3
AllInclusive ::=  SEQUENCE {
   -- Data Frame Items
   item6-1        AccelerationSet4Way                     OPTIONAL, 
   item6-2        AccelSteerYawRateConfidence             OPTIONAL,
   -- item6-3     AllInclusive                            OPTIONAL,
   item6-4        AntennaOffsetSet                        OPTIONAL,
   item6-5        Approach                                OPTIONAL,
   item6-6        ApproachObject                          OPTIONAL,
   item6-7        BarrierLane                             OPTIONAL,
   item6-8        BrakeSystemStatus                       OPTIONAL,
   item6-9        BSMblob                                 OPTIONAL,
   item6-10       BumperHeights                           OPTIONAL,
   item6-11       Circle                                  OPTIONAL,
   item6-12       ConfidenceSet                           OPTIONAL,
   item6-13       ConnectsTo                              OPTIONAL,
   item6-14       CrosswalkLane                           OPTIONAL,
   item6-15       DataParameters                          OPTIONAL,
   item6-16       DDate                                   OPTIONAL,
   item6-17       DDateTime                               OPTIONAL,
   item6-18       DFullTime                               OPTIONAL,
   item6-19       DMonthDay                               OPTIONAL,
   item6-20       DTime                                   OPTIONAL,
   item6-21       DYearMonth                              OPTIONAL,
   item6-22       FullPositionVector                      OPTIONAL,
   item6-23       Intersection                            OPTIONAL,
   item6-24       IntersectionState                       OPTIONAL,
   item6-25       ExitService                             OPTIONAL,
   item6-26       GenericSignage                          OPTIONAL,
   item6-27       SpeedLimit                              OPTIONAL,
   item6-28       WorkZone                                OPTIONAL,
   item6-29       J1939data                               OPTIONAL,
   item6-30       MovementState                           OPTIONAL,
   item6-31       NodeList                                OPTIONAL,
   item6-32       Offsets                                 OPTIONAL,
   item6-33       PathHistory                             OPTIONAL,
   item6-34       PathHistoryPointType-01                 OPTIONAL,
   item6-35       PathHistoryPointType-02                 OPTIONAL,
   item6-36       PathHistoryPointType-03                 OPTIONAL,
   item6-37       PathHistoryPointType-04                 OPTIONAL,
   item6-38       PathHistoryPointType-05                 OPTIONAL,
   item6-39       PathHistoryPointType-06                 OPTIONAL,
   item6-40       PathHistoryPointType-07                 OPTIONAL,
   item6-41       PathHistoryPointType-08                 OPTIONAL,
   item6-42       PathHistoryPointType-09                 OPTIONAL,
   item6-43       PathHistoryPointType-10                 OPTIONAL,
   item6-44       PathPrediction                          OPTIONAL,
   item6-45       Position3D                              OPTIONAL,
   item6-46       PositionalAccuracy                      OPTIONAL,
   item6-47       PositionConfidenceSet                   OPTIONAL,
   item6-48       RegionList                              OPTIONAL,
   item6-49       RegionOffsets                           OPTIONAL,
   item6-50       RegionPointSet                          OPTIONAL,
   item6-51       RoadSignID                              OPTIONAL,
   item6-52       RTCMHeader                              OPTIONAL,
   item6-53       RTCMmsg                                 OPTIONAL,
   item6-54       RTCMPackage                             OPTIONAL,
   item6-55       Sample                                  OPTIONAL,
   item6-56       ShapePointSet                           OPTIONAL,
   item6-57       SignalControlZone                       OPTIONAL,
   item6-58       SignalRequest                           OPTIONAL,
   item6-59       SnapshotDistance                        OPTIONAL,
   item6-60       Snapshot                                OPTIONAL,
   item6-61       SnapshotTime                            OPTIONAL,
   item6-62       SpecialLane                             OPTIONAL,
   item6-63       SpeedandHeadingandThrottleConfidence    OPTIONAL,
   item6-64       TransmissionAndSpeed                    OPTIONAL,
   item6-65       ValidRegion                             OPTIONAL,
   item6-66       VehicleComputedLane                     OPTIONAL,
   item6-67       VehicleIdent                            OPTIONAL,
   item6-68       VehicleReferenceLane                    OPTIONAL,
   item6-69       VehicleSafetyExtension                  OPTIONAL,
   item6-70       VehicleSize                             OPTIONAL,
   item6-71       VehicleStatusRequest                    OPTIONAL,
   item6-72       VehicleStatus                           OPTIONAL,
   item6-73       WiperStatus                             OPTIONAL,

   -- Data Element Items
   item7-1        Acceleration                            OPTIONAL,
   item7-2        AccelerationConfidence                  OPTIONAL,
   item7-3        AmbientAirPressure                      OPTIONAL,
   item7-4        AmbientAirTemperature                   OPTIONAL,
   item7-5        AntiLockBrakeStatus                     OPTIONAL,
   item7-6        ApproachNumber                          OPTIONAL,
   item7-7        AuxiliaryBrakeStatus                    OPTIONAL,
   item7-8        BarrierAttributes                       OPTIONAL,
   item7-9        BrakeAppliedPressure                    OPTIONAL,
   item7-10       BrakeAppliedStatus                      OPTIONAL,
   item7-11       BrakeBoostApplied                       OPTIONAL,
   item7-12       BumperHeightFront                       OPTIONAL,
   item7-13       BumperHeightRear                        OPTIONAL,
   item7-14       CodeWord                                OPTIONAL,
   item7-15       CoefficientOfFriction                   OPTIONAL,
   item7-16       ColorState                              OPTIONAL,
   item7-17       Count                                   OPTIONAL,
   item7-18       CrosswalkLaneAttributes                 OPTIONAL,
   item7-19       DDay                                    OPTIONAL,
   item7-20       DescriptiveName                         OPTIONAL,
   item7-21       DHour                                   OPTIONAL,
   item7-22       DirectionOfUse                          OPTIONAL,
   item7-23       DMinute                                 OPTIONAL,
   item7-24       DMonth                                  OPTIONAL,
   item7-25       DOffset                                 OPTIONAL,
   item7-26       DrivenLineOffset                        OPTIONAL,
   item7-27       DrivingWheelAngle                       OPTIONAL,
   item7-28       DSecond                                 OPTIONAL,
   item7-29       DSignalSeconds                          OPTIONAL,
   item7-30       DSRCmsgID                               OPTIONAL,
   item7-31       DYear                                   OPTIONAL,
   item7-32       ElevationConfidence                     OPTIONAL,
   item7-33       Elevation                               OPTIONAL,
   item7-34       EmergencyDetails                        OPTIONAL,
   item7-35       EventFlags                              OPTIONAL,
   item7-36       Extent                                  OPTIONAL,
   item7-37       ExteriorLights                          OPTIONAL,
   item7-38       FurtherInfoID                           OPTIONAL,
   item7-39       GPSstatus                               OPTIONAL,
   item7-40       HeadingConfidence                       OPTIONAL,
   item7-41       Heading                                 OPTIONAL,
   item7-42       HeadingSlice                            OPTIONAL,
   item7-43       IntersectionStatusObject                OPTIONAL,
   item7-44       IntersectionID                          OPTIONAL,
   item7-45       AxleLocation                            OPTIONAL,
   item7-46       AxleWeight                              OPTIONAL,
   item7-47       CargoWeight                             OPTIONAL,
   item7-48       DriveAxleLiftAirPressure                OPTIONAL,
   item7-49       DriveAxleLocation                       OPTIONAL,
   item7-50       DriveAxleLubePressure                   OPTIONAL,
   item7-51       DriveAxleTemperature                    OPTIONAL,
   item7-52       SteeringAxleLubePressure                OPTIONAL,
   item7-53       SteeringAxleTemperature                 OPTIONAL,
   item7-54       TireLeakageRate                         OPTIONAL,
   item7-55       TireLocation                            OPTIONAL,
   item7-56       TirePressureThresholdDetection          OPTIONAL,
   item7-57       TirePressure                            OPTIONAL,
   item7-58       TireTemp                                OPTIONAL,
   item7-59       TrailerWeight                           OPTIONAL,
   item7-60       WheelEndElectFault                      OPTIONAL,
   item7-61       WheelSensorStatus                       OPTIONAL,
   item7-62       LaneCount                               OPTIONAL,
   item7-63       LaneManeuverCode                        OPTIONAL,
   item7-64       LaneNumber                              OPTIONAL,
   item7-65       LaneSet                                 OPTIONAL,
   item7-66       LaneWidth                               OPTIONAL,
   item7-67       Latitude                                OPTIONAL,
   item7-68       LayerID                                 OPTIONAL,
   item7-69       LayerType                               OPTIONAL,
   item7-70       LightbarInUse                           OPTIONAL,
   item7-71       Longitude                               OPTIONAL,
   item7-72       Location-quality                        OPTIONAL,
   item7-73       Location-tech                           OPTIONAL,
   item7-74       MinuteOfTheYear                         OPTIONAL,
   item7-75       MinutesDuration                         OPTIONAL,
   item7-76       MsgCount                                OPTIONAL,
   item7-77       MsgCRC                                  OPTIONAL,
   item7-78       MultiVehicleResponse                    OPTIONAL,
   item7-79       MUTCDCode                               OPTIONAL,
   item7-80       NMEA-MsgType                            OPTIONAL,
   item7-81       NMEA-Payload                            OPTIONAL,
   item7-82       NMEA-Revision                           OPTIONAL,
   item7-83       NTCIPVehicleclass                       OPTIONAL,
   item7-84       ObjectCount                             OPTIONAL,
   item7-85       ObstacleDirection                       OPTIONAL,
   item7-86       ObstacleDistance                        OPTIONAL,
   item7-87       PayloadData                             OPTIONAL,
   item7-88       Payload                                 OPTIONAL,
   item7-89       PedestrianDetect                        OPTIONAL,
   item7-90       PedestrianSignalState                   OPTIONAL,
   item7-91       PositionConfidence                      OPTIONAL,
   item7-92       PreemptState                            OPTIONAL,
   item7-93       Priority                                OPTIONAL,
   item7-94       PriorityState                           OPTIONAL,
   item7-95       ProbeSegmentNumber                      OPTIONAL,
   item7-96       RainSensor                              OPTIONAL,
   item7-97       RequestedItem                           OPTIONAL,
   item7-98       ResponseType                            OPTIONAL,
   item7-99       RTCM-ID                                 OPTIONAL,
   item7-100      RTCM-Payload                            OPTIONAL,
   item7-101      RTCM-Revision                           OPTIONAL,
   item7-102      SignalLightState                        OPTIONAL,
   item7-103      SignalReqScheme                         OPTIONAL,
   item7-104      SignalState                             OPTIONAL,
   item7-105      SignPrority                             OPTIONAL,
   item7-106      SirenInUse                              OPTIONAL,
   item7-107      SpecialLaneAttributes                   OPTIONAL,
   item7-108      SpecialSignalState                      OPTIONAL,
   item7-109      SpeedConfidence                         OPTIONAL,
   item7-110      Speed                                   OPTIONAL,
   item7-111      StabilityControlStatus                  OPTIONAL,
   item7-112      StateConfidence                         OPTIONAL,
   item7-113      SteeringWheelAngleConfidence            OPTIONAL,
   item7-114      SteeringWheelAngleRateOfChange          OPTIONAL,
   item7-115      SteeringWheelAngle                      OPTIONAL,
   item7-116      SunSensor                               OPTIONAL,
   item7-117      TemporaryID                             OPTIONAL,
   item7-118      TermDistance                            OPTIONAL,
   item7-119      TermTime                                OPTIONAL,
   item7-120      ThrottleConfidence                      OPTIONAL,
   item7-121      ThrottlePosition                        OPTIONAL,
   item7-122      TimeConfidence                          OPTIONAL,
   item7-123      TimeMark                                OPTIONAL,
   item7-124      TractionControlState                    OPTIONAL,
   item7-125      TransitPreEmptionRequest                OPTIONAL,
   item7-126      TransitStatus                           OPTIONAL,
   item7-127      TransmissionState                       OPTIONAL,
   item7-128      TxTime                                  OPTIONAL,
   item7-129      TravelerInfoType                        OPTIONAL,
   item7-130      UniqueMSGID                             OPTIONAL,
   item7-131      URL-Base                                OPTIONAL,
   item7-132      URL-Link                                OPTIONAL,
   item7-133      URL-Short                               OPTIONAL,
   item7-134      VehicleHeight                           OPTIONAL,
   item7-135      VehicleLaneAttributes                   OPTIONAL,
   item7-136      VehicleLength                           OPTIONAL,
   item7-137      VehicleMass                             OPTIONAL,
   item7-138      VehicleRequestStatus                    OPTIONAL,
   item7-139      VehicleStatusDeviceTypeTag              OPTIONAL,
   item7-140      VehicleType                             OPTIONAL,
   item7-141      VehicleWidth                            OPTIONAL,
   item7-142      VerticalAccelerationThreshold           OPTIONAL,
   item7-143      VerticalAcceleration                    OPTIONAL,
   item7-144      VINstring                               OPTIONAL,
   item7-145      WiperRate                               OPTIONAL,
   item7-146      WiperStatusFront                        OPTIONAL,
   item7-147      WiperStatusRear                         OPTIONAL,
   item7-148      YawRateConfidence                       OPTIONAL,
   item7-149      YawRate                                 OPTIONAL,

   -- External Items
   item8-1        ITIS.IncidentResponseEquipment          OPTIONAL,
   item8-2        ITIS.ITIStext                           OPTIONAL,
   item8-3        ITIS.ResponderGroupAffected             OPTIONAL,
   item8-4        ITIS.VehicleGroupAffected               OPTIONAL,
   item8-5        ITIS.ITIScodesAndText                   OPTIONAL,
   item8-6        NTCIP.EssMobileFriction                 OPTIONAL,
   item8-7        NTCIP.EssPrecipRate                     OPTIONAL,
   item8-8        NTCIP.EssPrecipSituation                OPTIONAL,
   item8-9        NTCIP.EssPrecipYesNo                    OPTIONAL,
   item8-10       NTCIP.EssSolarRadiation                 OPTIONAL,
   item8-11       ITIS.ITIScodes                          OPTIONAL,
   ...
   }
 
 
 
-- DF_AntennaOffsetSet (Desc Name) Record 4
AntennaOffsetSet ::= OCTET STRING (SIZE(4)) 
   -- defined as:
   -- SEQUENCE {
   -- antOffsetX  INTEGER (-8191..8191), 
                  -- 14 bits in length
                  -- units of 1cm from center
                  -- 8191 to be used for unavailable 
   -- antOffsetY  INTEGER (-255..255),     
                  -- 9 bits in length
                  -- units of 1cm from center
                  -- 255 to be used for unavailable 
   -- antOffsetZ  INTEGER (0..511)        
                  -- 9 bits in length
                  -- units of 1cm from ground
                  -- 511 to be used for unavailable 
   -- }
 
 
 
-- DF_Approach (Desc Name) Record 5
Approach ::=  SEQUENCE {
   name           DescriptiveName OPTIONAL,
   id             ApproachNumber  OPTIONAL,    
   drivingLanes   SEQUENCE (SIZE(0..32)) OF
                  VehicleReferenceLane OPTIONAL, 
   computedLanes  SEQUENCE (SIZE(0..32)) OF 
                  VehicleComputedLane OPTIONAL, 
   trainsAndBuses SEQUENCE (SIZE(0..32)) OF 
                  SpecialLane OPTIONAL, 
   barriers       SEQUENCE (SIZE(0..32)) OF 
                  BarrierLane OPTIONAL, 
   crosswalks     SEQUENCE (SIZE(0..32)) OF 
                  CrosswalkLane OPTIONAL, 
   ... 
   }
 
 
 
-- DF_ApproachesObject (Desc Name) Record 6
ApproachObject ::=  SEQUENCE {
   refPoint     Position3D OPTIONAL, 
                -- optional reference from which subsequent 
                -- data points in this link are offset 
   laneWidth    LaneWidth  OPTIONAL, 
                -- reference width used by subsequent 
                -- lanes until a new width is given
   approach     Approach OPTIONAL, 
                -- list of Approaches and their lanes
   egress       Approach OPTIONAL, 
                -- list of Egresses and thier lanes
   ...
   }
 
 
 
-- DF_BarrierLane (Desc Name) Record 7
BarrierLane ::=  SEQUENCE {
   laneNumber          LaneNumber, 
   laneWidth           LaneWidth  OPTIONAL, 
   barrierAttributes   BarrierAttributes, 
   nodeList            NodeList,
                       -- path details of the Barrier
   ... 
   }
 
 
 
-- DF_BrakeSystemStatus (Desc Name) Record 8
BrakeSystemStatus ::= OCTET STRING (SIZE(2))
   -- Encoded with the packed content of: 
   -- SEQUENCE {
   --   wheelBrakes        BrakeAppliedStatus,
   --                      -x- 4 bits
   --   wheelBrakesUnavailable  BOOL
   --                      -x- 1 bit (1=true)
   --   spareBit
   --                      -x- 1 bit, set to zero
   --   traction           TractionControlState,
   --                      -x- 2 bits
   --   abs                AntiLockBrakeStatus, 
   --                      -x- 2 bits
   --   scs                StabilityControlStatus,
   --                      -x- 2 bits
   --   brakeBoost         BrakeBoostApplied, 
   --                      -x- 2 bits
   --   auxBrakes          AuxiliaryBrakeStatus,
   --                      -x- 2 bits
   --   }
 
 
 
-- DF_BSM_Blob (Desc Name) Record 9
BSMblob ::= OCTET STRING (SIZE(38)) 
   -- made up of the following 38 packed bytes:
   -- msgCnt      MsgCount,             -x- 1 byte
   -- id          TemporaryID,          -x- 4 bytes
   -- secMark     DSecond,              -x- 2 bytes

   -- lat         Latitude,             -x- 4 bytes 
   -- long        Longitude,            -x- 4 bytes
   -- elev        Elevation,            -x- 2 bytes
   -- accuracy    PositionalAccuracy,   -x- 4 bytes
   
   -- speed       TransmissionAndSpeed, -x- 2 bytes
   -- heading     Heading,              -x- 2 byte
   -- angle       SteeringWheelAngle    -x- 1 byte
   -- accelSet    AccelerationSet4Way,  -x- accel set (four way) 7 bytes
  
   -- brakes      BrakeSystemStatus,    -x- 2 bytes
   -- size        VehicleSize,          -x- 3 bytes
 
 
 
-- DF_BumperHeights (Desc Name) Record 10
BumperHeights ::=  SEQUENCE {
   frnt       BumperHeightFront, 
   rear       BumperHeightRear
   }
 
 
 
-- DF_Circle (Desc Name) Record 11
Circle ::=  SEQUENCE {
   center   Position3D,
   raduis   CHOICE {
               radiusSteps  INTEGER (0..32767), 
                              -- in unsigned values where 
                              -- the LSB is in units of 2.5 cm
               miles        INTEGER (1..2000),
               km           INTEGER (1..5000)
            } --# UNTAGGED
   }
 
 
 
-- DF_ConfidenceSet (Desc Name) Record 12
ConfidenceSet ::=  SEQUENCE {
   accelConfidence      AccelSteerYawRateConfidence OPTIONAL,    
   speedConfidence      SpeedandHeadingandThrottleConfidence OPTIONAL,      
   timeConfidence       TimeConfidence OPTIONAL,           
   posConfidence        PositionConfidenceSet OPTIONAL,           
   steerConfidence      SteeringWheelAngleConfidence OPTIONAL,
   throttleConfidence   ThrottleConfidence OPTIONAL,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_ConnectsTo (Desc Name) Record 13
ConnectsTo ::= OCTET STRING (SIZE(2..32))
   -- sets of 2 byte pairs,
   -- the first byte is a lane number
   -- the second byte is a LaneManeuverCode
 
 
 
-- DF_CrosswalkLane (Desc Name) Record 14
CrosswalkLane ::=  SEQUENCE {
   laneNumber       LaneNumber, 
   laneWidth        LaneWidth  OPTIONAL, 
   laneAttributes   CrosswalkLaneAttributes, 
   nodeList         NodeList,
                    -- path details of the lane
                    -- note that this may cross or pass 
                    -- by driven lanes
   keepOutList      NodeList OPTIONAL,
                    -- no stop points along the path 
                    -- typically the end points unless
                    -- islands are represented in the path
   connectsTo       ConnectsTo  OPTIONAL, 
                    -- a list of other lanes and their
                    -- turning use by this lane
   ... 
   }
 
 
 
-- DF_DataParameters (Desc Name) Record 15
DataParameters ::=  SEQUENCE {
   processMethod     IA5String(SIZE(1..255)) OPTIONAL, 
   processAgency     IA5String(SIZE(1..255)) OPTIONAL, 
   lastCheckedDate   IA5String(SIZE(1..255)) OPTIONAL, 
   geiodUsed         IA5String(SIZE(1..255)) OPTIONAL, 
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_DDate (Desc Name) Record 16
DDate ::=  SEQUENCE {
   year    DYear,            -- 2 bytes
   month   DMonth,           -- 1 byte
   day     DDay              -- 1 byte
   }
 
 
 
-- DF_DDateTime (Desc Name) Record 17
DDateTime ::=  SEQUENCE {
   year    DYear    OPTIONAL,    -- 2 bytes
   month   DMonth   OPTIONAL,    -- 1 byte
   day     DDay     OPTIONAL,    -- 1 byte
   hour    DHour    OPTIONAL,    -- 1 byte
   minute  DMinute  OPTIONAL,    -- 1 byte
   second  DSecond  OPTIONAL     -- 2 bytes
   }
 
 
 
-- DF_DFullTime (Desc Name) Record 18
DFullTime ::=  SEQUENCE {
   year    DYear,            -- 2 bytes
   month   DMonth,           -- 1 byte
   day     DDay,             -- 1 byte
   hour    DHour,            -- 1 byte
   minute  DMinute           -- 1 byte
   }
 
 
 
-- DF_DMonthDay (Desc Name) Record 19
DMonthDay ::=  SEQUENCE {
   month   DMonth,           -- 1 byte
   day     DDay              -- 1 byte
   }
 
 
 
-- DF_DTime (Desc Name) Record 20
DTime ::=  SEQUENCE {
   hour    DHour,            -- 1 byte
   minute  DMinute,          -- 1 byte
   second  DSecond           -- 2 bytes
   }
 
 
 
-- DF_DYearMonth (Desc Name) Record 21
DYearMonth ::=  SEQUENCE {
   year    DYear,            -- 2 bytes
   month   DMonth            -- 1 byte
   }
 
 
 
-- DF_ITIS_Phrase_ExitService (Desc Name) Record 22
ExitService ::=  SEQUENCE (SIZE(1..10)) OF SEQUENCE {
  item CHOICE    {
       itis   ITIS.ITIScodes,
       text   IA5String  (SIZE(1..16))
       } -- # UNTAGGED
  }
 
 
 
-- DF_FullPositionVector (Desc Name) Record 23
FullPositionVector ::=  SEQUENCE {
   utcTime             DDateTime OPTIONAL,   -- time with mSec precision
   long                Longitude,            -- 1/10th microdegree
   lat                 Latitude,             -- 1/10th microdegree
   elevation           Elevation OPTIONAL,   -- 3 bytes, 0.1 m
   heading             Heading OPTIONAL, 
   speed               TransmissionAndSpeed OPTIONAL,
   posAccuracy         PositionalAccuracy OPTIONAL,
   timeConfidence      TimeConfidence OPTIONAL,
   posConfidence       PositionConfidenceSet OPTIONAL,
   speedConfidence     SpeedandHeadingandThrottleConfidence OPTIONAL, 
   ... -- # LOCAL_CONTENT 
   }
 
 
 
-- DF_ITIS_Phrase_GenericSignage (Desc Name) Record 24
GenericSignage ::=  SEQUENCE (SIZE(1..10)) OF SEQUENCE {
  item CHOICE    {
       itis   ITIS.ITIScodes,
       text   IA5String  (SIZE(1..16))
       } -- # UNTAGGED
  }
 
 
 
-- DF_Intersection (Desc Name) Record 25
Intersection ::=  SEQUENCE {
   name        DescriptiveName OPTIONAL,
   id          IntersectionID,      
                                  -- a gloablly unique value, 
                                  -- the upper bytes of which may not 
                                  -- be sent if the context is known
   refPoint    Position3D OPTIONAL,      
                                  -- the reference from which subsequent 
                                  -- data points are offset until a new
                                  -- point is used. 
   refInterNum IntersectionID OPTIONAL, 
                                  -- present only if this is a computed
                                  -- intersection instance     
   orientation Heading OPTIONAL, 
                                  -- present only if this is a computed
                                  -- intersection instance     

   laneWidth   LaneWidth  OPTIONAL, 
                                  -- reference width used by subsequent 
                                  -- lanes until a new width is given
   type        IntersectionStatusObject OPTIONAL, 
                                  -- data about the intersection type
   approaches SEQUENCE (SIZE(1..32)) OF 
               ApproachObject,      
                                  -- data about one or more approaches 
                                  -- (lane data is found here)
   preemptZones SEQUENCE (SIZE(1..32)) OF 
               SignalControlZone OPTIONAL, 
                                  -- data about one or more
                                  -- preempt zones 
   priorityZones SEQUENCE (SIZE(1..32)) OF 
               SignalControlZone OPTIONAL,    
                                  -- data about one or more 
                                  -- priority zones
   ... 
   }
 
 
 
-- DF_IntersectionState (Desc Name) Record 26
IntersectionState ::=  SEQUENCE {
   name        DescriptiveName OPTIONAL, 
               -- human readable name for intersection  
               -- to be used only in debug mode
   id          IntersectionID, 
               -- this provided a unique mapping to the 
               -- intersection map in question
               -- which provides complete location 
               -- and approach/move/lane data
   status      IntersectionStatusObject,
               -- general status of the controller
   timeStamp   TimeMark OPTIONAL, 
               -- the point in local time that
               -- this message was constructed
   lanesCnt    INTEGER(1..255) OPTIONAL,  
               -- number of states to follow (not always
               -- one per lane because sign states may be shared)
   states      SEQUENCE (SIZE(1..255)) OF MovementState,
               -- each active Movement/lane is given in turn
               -- and contains its state, and seconds
               -- to the next event etc. 
   priority    SignalState OPTIONAL,
               -- the active priority state data, if present
   preempt     SignalState OPTIONAL,
               -- the active preemption state data, if present
   
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_J1939-Data Items (Desc Name) Record 27
J1939data ::=  SEQUENCE {
   -- Tire conditions 
   tires SEQUENCE (SIZE(0..16)) OF SEQUENCE {
      location              TireLocation              OPTIONAL, 
      pressure              TirePressure              OPTIONAL, 
      temp                  TireTemp                  OPTIONAL, 
      wheelSensorStatus     WheelSensorStatus         OPTIONAL, 
      wheelEndElectFault    WheelEndElectFault        OPTIONAL, 
      leakageRate           TireLeakageRate           OPTIONAL, 
      detection             TirePressureThresholdDetection OPTIONAL, 
      ...
      } OPTIONAL,      
   -- Vehicle Weight by axle
   axle SEQUENCE (SIZE(0..16)) OF SEQUENCE {
      location              AxleLocation              OPTIONAL, 
      weight                AxleWeight                OPTIONAL, 
      ...
      } OPTIONAL,      
   trailerWeight            TrailerWeight             OPTIONAL,     
   cargoWeight              CargoWeight               OPTIONAL, 
   steeringAxleTemperature  SteeringAxleTemperature   OPTIONAL, 
   driveAxleLocation        DriveAxleLocation         OPTIONAL, 
   driveAxleLiftAirPressure DriveAxleLiftAirPressure  OPTIONAL, 
   driveAxleTemperature     DriveAxleTemperature      OPTIONAL, 
   driveAxleLubePressure    DriveAxleLubePressure     OPTIONAL, 
   steeringAxleLubePressure SteeringAxleLubePressure  OPTIONAL, 
   ...
   }
 
 
 
-- DF_MovementState (Desc Name) Record 28
MovementState ::=  SEQUENCE {
   -- The MovementNumber is contained in the enclosing DF. 
   movementName     DescriptiveName OPTIONAL,
                    -- uniquely defines movement by name   
   laneCnt          LaneCount OPTIONAL,
                    -- the number of lanes to follow
   laneSet          LaneSet,
                    -- each encoded as a LaneNumber, 
                    -- the collection of lanes, by num, 
                    -- to which this state data applies 
  -- For the current movement State, you may CHOICE one of the below:
          currState     SignalLightState OPTIONAL,    
                        -- the state of a Motorised lane
          pedState      PedestrianSignalState OPTIONAL,
                        -- the state of a Pedestrian type lane
          specialState  SpecialSignalState OPTIONAL,
                        -- the state of a special type lane
                        -- such as a dedicated train lane
   
   timeToChange     TimeMark,  
                    -- the point in time this state will change 
   stateConfidence  StateConfidence  OPTIONAL,
   
   -- Yellow phase time intervals 
   -- (used for motorised vehicle lanes and pedestrian lanes)
   -- For the yellow Signal State, a CHOICE of one of the below:
           yellState     SignalLightState OPTIONAL,   
                         -- the next state of a 
                         -- Motorised lane
           yellPedState  PedestrianSignalState OPTIONAL,
                         -- the next state of a 
                         -- Pedestrian type lane
   
   yellTimeToChange     TimeMark  OPTIONAL,    
   yellStateConfidence  StateConfidence  OPTIONAL,
   
   -- below items are all optional based on use and context 
   -- some are used only for ped lane types
   vehicleCount  ObjectCount OPTIONAL, 
   pedDetect     PedestrianDetect  OPTIONAL,
                 -- true if ANY ped are detected crossing 
                 -- the above lanes
   pedCount      ObjectCount OPTIONAL,  
                 -- est count of peds
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_NodeList (Desc Name) Record 29
NodeList ::=  SEQUENCE (SIZE(1..64)) OF Offsets
   -- the Position3D ref point (starting point or anchor)
   -- is found in the outer object. 
   -- Offsets are additive from the last point.
 
 
 
-- DF_Offsets (Desc Name) Record 30
Offsets ::= OCTET STRING (SIZE(4..8)) 
   -- Made up of 
   -- SEQUENCE {
   -- xOffset  INTEGER (-32767..32767), 
   -- yOffset  INTEGER (-32767..32767),
   -- if 6 or 8 bytes in length:
   -- zOffset  INTEGER (-32767..32767) OPTIONAL,
            -- all above in signed values where 
            -- the LSB is in units of 1.0 cm   
  
   -- if 8 bytes in length:
   -- width    LaneWidth               OPTIONAL
   -- a length of 7 bytes is never used
   -- }
 
 
 
-- DF_PathHistory (Desc Name) Record 31
PathHistory ::=  SEQUENCE {
   initialPosition  FullPositionVector   OPTIONAL,
   currGPSstatus    GPSstatus            OPTIONAL,   
   itemCnt          Count                OPTIONAL,   
                    -- Limited to range 1 to 23
                    -- number of points in set to follow
   crumbData  CHOICE {
              -- select one of the possible data sets to be used
   
              pathHistoryPointSets-01 SEQUENCE (SIZE(1..23)) OF 
                    PathHistoryPointType-01,
                    -- made up of sets of the: PathHistoryPointType-1
                    -- a set of all data elements, it is 
                    -- non-uniform in size, each item tagged in BER
   
              pathHistoryPointSets-02 OCTET STRING (SIZE(15..345)), 
                    -- made up of sets of the: PathHistoryPointType-02
                    -- sets of all data elements including:
                    -- lat, long, elev, time, accuracy, heading, and speed
                    -- offsets sent as a packed blob of 15 bytes per point
           
              pathHistoryPointSets-03 OCTET STRING (SIZE(12..276)), 
                    -- made up of sets of the: PathHistoryPointType-03
                    -- sets of the following data elements:
                    -- lat, long, elev, time, and accuracy
                    -- offsets sent as a packed blob of 12 bytes per point
  
              pathHistoryPointSets-04 OCTET STRING (SIZE(8..184)), 
                    -- made up of sets of the: PathHistoryPointType-04
                    -- sets of the following data elements:
                    -- lat, long, elev, and time
                    -- offsets sent as a packed blob of 8 bytes per point
   
              pathHistoryPointSets-05 OCTET STRING (SIZE(10..230)), 
                    -- made up of sets of the: PathHistoryPointType-05
                    -- sets of the following data elements:
                    -- lat, long, elev, and accuracy
                    -- offsets sent as a packed blob of 10 bytes per point
   
              pathHistoryPointSets-06 OCTET STRING (SIZE(6..138)), 
                    -- made up of sets of the: PathHistoryPointType-06
                    -- sets of the following data elements:
                    -- lat, long, and elev
                    -- offsets sent as a packed blob of 6 bytes per point
   
              pathHistoryPointSets-07 OCTET STRING (SIZE(11..242)), 
                    -- made up of sets of the: PathHistoryPointType-07
                    -- sets of the following data elements:
                    -- lat, long, time, and accuracy
                    -- offsets sent as a packed blob of 10.5 bytes per point
   
              pathHistoryPointSets-08 OCTET STRING (SIZE(7..161)), 
                    -- made up of sets of the: PathHistoryPointType-08
                    -- sets of the following data elements:
                    -- lat, long, and time
                    -- offsets sent as a packed blob of 7 bytes per point
   
              pathHistoryPointSets-09 OCTET STRING (SIZE(9..196)), 
                    -- made up of sets of the: PathHistoryPointType-09
                    -- sets of the following data elements:
                    -- lat, long, and accuracy
                    -- offsets sent as a packed blob of 8.5 bytes per point
   
              pathHistoryPointSets-10 OCTET STRING (SIZE(5..104))   
                    -- made up of sets of the: PathHistoryPointType-10
                    -- sets of the following data elements:
                    -- lat and long
                    -- offsets sent as a packed blob of 4.5 bytes per point

              }, 
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_PathHistoryPointType-01 (Desc Name) Record 32
PathHistoryPointType-01 ::=  SEQUENCE {
   latOffset  INTEGER (-131072..131071), 
               -- in 1/10th micro degrees 
               -- value  131071 to be used for  131071 or greater
               -- value -131071 to be used for -131071 or less
               -- value -131072 to be used for unavailable lat or long

   longOffset INTEGER (-131072..131071), 
               -- in 1/10th micro degrees 
               -- value  131071 to be used for  131071 or greater
               -- value -131071 to be used for -131071 or less
               -- value -131072 to be used for unavailable lat or long

    elevationOffset INTEGER (-2048..2047) OPTIONAL,  
               -- LSB units of of 10 cm
               -- value  2047 to be used for  2047 or greater
               -- value -2047 to be used for -2047 or greater
               -- value -2048 to be unavailable

    timeOffset INTEGER (1..65535) OPTIONAL,
               -- LSB units of of 10 mSec  
               -- value  65534 to be used for 65534 or greater
               -- value  65535 to be unavailable

    posAccuracy PositionalAccuracy OPTIONAL, 
               -- four packed bytes

    heading    INTEGER (-128..127) OPTIONAL, 
               -- where the LSB is in 
               -- units of 1.5 degrees 
               -- value -128 for unavailable
               -- not an offset value

    speed      TransmissionAndSpeed  OPTIONAL 
               -- upper bits encode transmission 
               -- where the LSB is in 
               -- units of 0.02 m/s
               -- not an offset value 
   }
 
 
 
-- DF_PathHistoryPointType-02 (Desc Name) Record 33
PathHistoryPointType-02 ::= OCTET STRING (SIZE(15)) 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- elevationOffset    INTEGER (-2048..2047), (12 signed bits)
    -- LSB units of 10 cm
    -- value  2047 to be used for  2047 or greater
    -- value -2047 to be used for -2047 or greater
    -- value -2048 to be unavailable

   -- timeOffset INTEGER (0..65535), (16 unsigned bits)
    -- LSB units of of 10 mSec   
    -- value  65534 to be used for 65534 or greater
    -- value  65535 to be unavailable

   -- accuracy   PositionalAccuracy 
    -- four packed bytes

   -- heading    INTEGER (-128..127), (8 signed bits)
    -- where the LSB is in 
    -- units of 1.5 degrees 
    -- value -128 for unavailable
    -- not an offset value

   -- speed      TransmissionAndSpeed (16 encoded bits) 
    -- upper bits encode transmission 
    -- where the LSB is in 
    -- units of 0.02 m/s
    -- not an offset value
 
 
 
-- DF_PathHistoryPointType-03 (Desc Name) Record 34
PathHistoryPointType-03 ::= OCTET STRING (SIZE(12)) 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- elevationOffset    INTEGER (-2048..2047), (12 signed bits)
    -- LSB units of 10 cm
    -- value  2047 to be used for  2047 or greater
    -- value -2047 to be used for -2047 or greater
    -- value -2048 to be unavailable

   -- timeOffset INTEGER (0..65535), (16 unsigned bits)
    -- LSB units of of 10 mSec   
    -- value  65534 to be used for 65534 or greater
    -- value  65535 to be unavailable

   -- accuracy   PositionalAccuracy 
    -- four packed bytes
 
 
 
-- DF_PathHistoryPointType-04 (Desc Name) Record 35
PathHistoryPointType-04 ::= OCTET STRING (SIZE(8)) 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- elevationOffset    INTEGER (-2048..2047), (12 signed bits)
    -- LSB units of 10 cm
    -- value  2047 to be used for  2047 or greater
    -- value -2047 to be used for -2047 or greater
    -- value -2048 to be unavailable

   -- timeOffset INTEGER (0..65535), (16 unsigned bits)
    -- LSB units of of 10 mSec   
    -- value  65534 to be used for 65534 or greater
    -- value  65535 to be unavailable
 
 
 
-- DF_PathHistoryPointType-05 (Desc Name) Record 36
PathHistoryPointType-05 ::= OCTET STRING (SIZE(10)) 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- elevationOffset    INTEGER (-2048..2047), (12 signed bits)
    -- LSB units of 10 cm
    -- value  2047 to be used for  2047 or greater
    -- value -2047 to be used for -2047 or greater
    -- value -2048 to be unavailable

   -- accuracy   PositionalAccuracy 
    -- four packed bytes
 
 
 
-- DF_PathHistoryPointType-06 (Desc Name) Record 37
PathHistoryPointType-06 ::= OCTET STRING (SIZE(6)) 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- elevationOffset    INTEGER (-2048..2047), (12 signed bits)
    -- LSB units of 10 cm
    -- value  2047 to be used for  2047 or greater
    -- value -2047 to be used for -2047 or greater
    -- value -2048 to be unavailable
 
 
 
-- DF_PathHistoryPointType-07 (Desc Name) Record 38
PathHistoryPointType-07 ::= OCTET STRING (SIZE(11)) -- in fact 10.5
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- timeOffset INTEGER (0..65535), (16 unsigned bits)
    -- LSB units of of 10 mSec   
    -- value  65534 to be used for 65534 or greater
    -- value  65535 to be unavailable

   -- accuracy   PositionalAccuracy 
    -- four packed bytes
 
 
 
-- DF_PathHistoryPointType-08 (Desc Name) Record 39
PathHistoryPointType-08 ::= OCTET STRING (SIZE(7)) -- in fact 6.5 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- timeOffset INTEGER (0..65535), (16 unsigned bits)
    -- LSB units of of 10 mSec   
    -- value  65534 to be used for 65534 or greater
    -- value  65535 to be unavailable
 
 
 
-- DF_PathHistoryPointType-09 (Desc Name) Record 40
PathHistoryPointType-09 ::= OCTET STRING (SIZE(9)) -- in fact 8.5
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long

   -- accuracy   PositionalAccuracy 
    -- four packed bytes
 
 
 
-- DF_PathHistoryPointType-10 (Desc Name) Record 41
PathHistoryPointType-10 ::= OCTET STRING (SIZE(5))   -- in fact 4.5 
   -- To be made up of packed bytes as follows: 
   -- latOffset  INTEGER (-131072..131071) (18 signed bits)
   -- longOffset INTEGER (-131072..131071) (18 signed bits)
    -- in 1/10th micro degrees 
    -- value  131071 to be used for  131071 or greater
    -- value -131071 to be used for -131071 or less
    -- value -131072 to be used for unavailable lat or long
 
 
 
-- DF_PathPrediction (Desc Name) Record 42
PathPrediction ::=  SEQUENCE {
   radiusOfCurve INTEGER (-32767..32767), 
                 -- LSB units of 10cm
                 -- straight path to use value of 32767
   confidence    INTEGER (0..200), 
                 -- LSB units of 0.5 percent
  
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_Position3D (Desc Name) Record 43
Position3D ::=  SEQUENCE {
   lat         Latitude,   -- in 1/10th micro degrees
   long        Longitude,  -- in 1/10th micro degrees
   elevation   Elevation   OPTIONAL  
   }
 
 
 
-- DF_PositionalAccuracy (Desc Name) Record 44
PositionalAccuracy ::= OCTET STRING (SIZE(4)) 
  -- And the bytes defined as folllows

  -- Byte 1: semi-major accuracy at one standard dev 
  -- range 0-12.7 meter, LSB = .05m
  -- 0xFE=254=any value equal or greater than 12.70 meter
  -- 0xFF=255=unavailable semi-major value 

  -- Byte 2: semi-minor accuracy at one standard dev 
  -- range 0-12.7 meter, LSB = .05m
  -- 0xFE=254=any value equal or greater than 12.70 meter
  -- 0xFF=255=unavailable semi-minor value 

  -- Bytes 3-4: orientation of semi-major axis 
  -- relative to true north (0~359.9945078786 degrees)
  -- LSB units of 360/65535 deg  = 0.0054932479
  -- a value of 0x0000 =0 shall be 0 degrees
  -- a value of 0x0001 =1 shall be 0.0054932479degrees 
  -- a value of 0xFFFE =65534 shall be 359.9945078786 deg
  -- a value of 0xFFFF =65535 shall be used for orientation unavailable 
  -- (In NMEA GPGST)
 
 
 
-- DF_PositionConfidenceSet (Desc Name) Record 45
PositionConfidenceSet ::= OCTET STRING (SIZE(1)) 
-- To be encoded as:
-- SEQUENCE {
--   pos        PositionConfidence, 
--              -x- 4 bits, for both horizontal directions
--   elevation  ElevationConfidence 
--              -x- 4 bits
--   }
 
 
 
-- DF_RegionList (Desc Name) Record 46
RegionList ::=  SEQUENCE (SIZE(1..64)) OF RegionOffsets
   -- the Position3D ref point (starting point or anchor)
   -- is found in the outer object.
 
 
 
-- DF_RegionOffsets (Desc Name) Record 47
RegionOffsets ::=  SEQUENCE {
   xOffset  INTEGER (-32767..32767), 
   yOffset  INTEGER (-32767..32767),
   zOffset  INTEGER (-32767..32767) OPTIONAL
            -- all in signed values where 
            -- the LSB is in units of 1 meter  
   }
 
 
 
-- DF_RegionPointSet (Desc Name) Record 48
RegionPointSet ::=  SEQUENCE {
   anchor           Position3D  OPTIONAL, 
   nodeList         RegionList,      
                    -- path details of the regions outline        
   ...             
   }
 
 
 
-- DF_RoadSignID (Desc Name) Record 49
RoadSignID ::=  SEQUENCE {
   position        Position3D,
                   -- Location of sign
   viewAngle       HeadingSlice,
                   -- Vehicle direction of travel while
                   -- facing active side of sign
   mutcdCode       MUTCDCode OPTIONAL,
                   -- Tag for MUTCD code or "generic sign" 
   crc             MsgCRC OPTIONAL 
                   -- Used to provide a check sum
   }
 
 
 
-- DF_RTCMHeader (Desc Name) Record 50
RTCMHeader ::= OCTET STRING (SIZE(5))
   -- defined as:
   -- SEQUENCE {
   -- status     GPSstatus,
                 -- to occupy 1 byte 
   -- offsetSet  AntennaOffsetSet
                 -- to occupy 4 bytes 
   -- }
 
 
 
-- DF_RTCMmsg (Desc Name) Record 51
RTCMmsg ::=  SEQUENCE {
   rev       RTCM-Revision OPTIONAL,
   rtcmID    RTCM-ID       OPTIONAL,
             -- the message and sub-message type, as
             -- defined in the RTCM revision being used
   payload   RTCM-Payload,
             -- the payload bytes
   ... -- # LOCAL_CONTENT   
   }
 
 
 
-- DF_RTCMPackage (Desc Name) Record 52
RTCMPackage ::=  SEQUENCE {
   anchorPoint FullPositionVector OPTIONAL,
   -- precise observer position, if needed
    
   rtcmHeader  RTCMHeader,
   -- an octet blob consisting of: 
   -- one byte with:
   -- GPSstatus
   -- 4 bytes with:                 
   -- AntennaOffsetSet  containing x,y,z data         
          
   -- note that a max of 16 satellites are allowed
   msg1001    OCTET STRING (SIZE(16..124))  OPTIONAL, 
              -- pRange data GPS L1
   msg1002    OCTET STRING (SIZE(18..156))  OPTIONAL, 
              -- pRange data GPS L1

   msg1003    OCTET STRING (SIZE(21..210))  OPTIONAL, 
               -- pRange data GPS L1, L2
   msg1004    OCTET STRING (SIZE(24..258))  OPTIONAL, 
               -- pRange data GPS L1, L2

   msg1005    OCTET STRING (SIZE(19))  OPTIONAL, 
              -- observer station data
   msg1006    OCTET STRING (SIZE(21))  OPTIONAL, 
              -- observer station data

   msg1007    OCTET STRING (SIZE(5..36))  OPTIONAL, 
              -- antenna of observer station data
   msg1008    OCTET STRING (SIZE(6..68))  OPTIONAL, 
              -- antenna of observer station data
 
   msg1009    OCTET STRING (SIZE(16..136))  OPTIONAL, 
               -- pRange data GLONASS L1
   msg1010    OCTET STRING (SIZE(18..166))  OPTIONAL, 
               -- pRange data GLONASS L1

   msg1011    OCTET STRING (SIZE(21..222))  OPTIONAL, 
               -- pRange data GLONASS L1, L2
   msg1012    OCTET STRING (SIZE(24..268))  OPTIONAL, 
               -- pRange data GLONASS L1, L2

   msg1013    OCTET STRING (SIZE(13..27))  OPTIONAL, 
               -- system parameters data

   ..., -- # LOCAL_CONTENT
   -- The below items shall never be sent 
   -- over WSM stack encoding (other encodings may be used)
   -- and may be removed from the ASN 

   msg1014    OCTET STRING (SIZE(15))  OPTIONAL, 
              -- Network Aux Station (NAS) data
   msg1015    OCTET STRING (SIZE(13..69))  OPTIONAL, 
              -- Ionospheric Correction data
   msg1016    OCTET STRING (SIZE(14..81))  OPTIONAL, 
              -- Geometry Correction data
   msg1017    OCTET STRING (SIZE(16..115))  OPTIONAL, 
              -- Combined Ionospheric and Geometry data

   -- msg1018 is reserved at this time

   msg1019    OCTET STRING (SIZE(62))  OPTIONAL, 
              -- Satellite Ephermeris data
   msg1020    OCTET STRING (SIZE(45))  OPTIONAL, 
              -- Satellite Ephermeris data
   msg1021    OCTET STRING (SIZE(62))  OPTIONAL, 
              -- Helmert-Abridged Molodenski Transform data
   msg1022    OCTET STRING (SIZE(75))  OPTIONAL, 
              -- Molodenski-Badekas Transform data
   msg1023    OCTET STRING (SIZE(73))  OPTIONAL, 
              -- Ellipse Residuals data
   msg1024    OCTET STRING (SIZE(74))  OPTIONAL, 
              -- Plane-Grid Residuals data
   msg1025    OCTET STRING (SIZE(25))  OPTIONAL, 
              -- Non-Lab Conic Project data
   msg1026    OCTET STRING (SIZE(30))  OPTIONAL, 
              -- Lab Conic Conform Project data
   msg1027    OCTET STRING (SIZE(33))  OPTIONAL, 
              -- Ob Mercator Project data

   -- msg1028 is reserved at this time

   msg1029    OCTET STRING (SIZE(10..69))  OPTIONAL, 
              -- Unicode test type data
   msg1030    OCTET STRING (SIZE(14..105))  OPTIONAL, 
              -- GPS Residuals data
   msg1031    OCTET STRING (SIZE(15..107))  OPTIONAL, 
              -- GLONASS Residuals data
   msg1032    OCTET STRING (SIZE(20))  OPTIONAL, 
              -- Ref Station Position data

   -- Proprietary Data content (msg40xx to msg4095) 
   -- may be added as needed 

   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_Sample (Desc Name) Record 53
Sample ::=  SEQUENCE {
   sampleStart  INTEGER(0..255),   --  Sample Starting Point
   sampleEnd    INTEGER(0..255)    --  Sample Ending Point
   }
 
 
 
-- DF_ShapePointSet (Desc Name) Record 54
ShapePointSet ::=  SEQUENCE {
   anchor           Position3D     OPTIONAL, 
   laneWidth        LaneWidth      OPTIONAL, 
   directionality   DirectionOfUse OPTIONAL, 
   nodeList         NodeList,      -- path details of the lane and width           
   ...             
   }
 
 
 
-- DF_SignalControlZone (Desc Name) Record 55
SignalControlZone ::=  SEQUENCE {
   name           DescriptiveName OPTIONAL,
                  -- used only for debugging 
   pValue         SignalReqScheme, 
                  -- preempt or priorty value (0..7),
                  -- and any strategy value to be used
   
   data           CHOICE {
   
         laneSet  SEQUENCE (SIZE(1..32)) OF LaneNumber, 
                  -- a seq of of defined LaneNumbers,
                  -- to be used with this p value
                  -- see thier nodelists for paths
   
         zones    SEQUENCE (SIZE(1..32)) OF SEQUENCE {
   
             enclosed  SEQUENCE (SIZE(1..32)) OF LaneNumber OPTIONAL, 
                              -- lanes in this region
             laneWidth        LaneWidth  OPTIONAL, 
             nodeList         NodeList,
                              -- path details of 
                              -- the region starting from 
                              -- the stop line
             ... 
             } 
             -- Note: unlike a nodelist for lanes, 
             -- zones may overlap by a considerable degree
        },
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_SignalRequest (Desc Name) Record 56
SignalRequest ::=  SEQUENCE {
   -- the regionally unique ID of the target intersection 
   id     IntersectionID,  --  intersection ID
   
   -- Below present only when canceling a prior request
   isCancel   SignalReqScheme OPTIONAL,  
   
   -- In typical use either a SignalReqScheme  
   -- or a lane number would be given, this
   -- indicates the scheme to use or the 
   -- path through the intersection
   -- to the degree it is known.
   -- Note that SignalReqScheme can hold either
   -- a preempt or a priority value. 
   requestedAction  SignalReqScheme OPTIONAL,  
                           --  preempt ID or the
                           --  priority ID 
                           --  (and strategy) 
   inLane      LaneNumber OPTIONAL,       
                           --  approach Lane
   outLane     LaneNumber OPTIONAL,       
                           --  egress Lane
   type        NTCIPVehicleclass, 
                           --  Two 4 bit nibbles as: 
                           --  NTCIP vehicle class type
                           --  NTCIP vehicle class level
   
   -- any validation string used by the system
   codeWord    CodeWord OPTIONAL,
   ...
   }
 
 
 
-- DF_Snapshot (Desc Name) Record 57
Snapshot ::=  SEQUENCE {
   thePosition  FullPositionVector,          
                -- data of the position and speed, 
   safetyExt    VehicleSafetyExtension OPTIONAL, 
   datSet       VehicleStatus          OPTIONAL,  
                -- a seq of data frames
                -- which encodes the data
  ... -- # LOCAL_CONTENT 
   }
 
 
 
-- DF_SnapshotDistance (Desc Name) Record 58
SnapshotDistance ::=  SEQUENCE {
   d1   INTEGER(0..999),   -- meters
   s1   INTEGER(0..50),    -- meters\second
   d2   INTEGER(0..999),   -- meters
   s2   INTEGER(0..50)     -- meters\second
   }
 
 
 
-- DF_SnapshotTime (Desc Name) Record 59
SnapshotTime ::=  SEQUENCE {
   t1   INTEGER(1..99),   
        -- m/sec - the instantaneous speed when the 
        -- calculation is performed
   s1   INTEGER(0..50),   
        -- seconds
   t2   INTEGER(1..99),  
        -- m/sec - the instantaneous speed when the 
        -- calculation is performed
   s2   INTEGER(0..50)    
        -- seconds
   }
 
 
 
-- DF_SpecialLane (Desc Name) Record 60
SpecialLane ::=  SEQUENCE {
   laneNumber       LaneNumber, 
   laneWidth        LaneWidth  OPTIONAL, 
   laneAttributes   SpecialLaneAttributes, 
   nodeList         NodeList,
                    -- path details of the lane and width
   keepOutList      NodeList OPTIONAL, 
                    -- no stop points along the path 
   connectsTo       ConnectsTo  OPTIONAL, 
                    -- a list of other lanes and their
                    -- turning use by this lane
   ... 
   }
 
 
 
-- DF_Speed_Heading_Throttle_Confidence (Desc Name) Record 61
SpeedandHeadingandThrottleConfidence ::= OCTET STRING (SIZE(1)) 
-- to be packed as follows: 
-- SEQUENCE {
--   heading   HeadingConfidence,   -x- 3 bits 
--   speed     SpeedConfidence,     -x- 3 bits 
--   throttle  ThrottleConfidence   -x- 2 bits
--   }
 
 
 
-- DF_ITIS_Phrase_SpeedLimit (Desc Name) Record 62
SpeedLimit ::=  SEQUENCE (SIZE(1..10)) OF SEQUENCE {
  item CHOICE    {
       itis   ITIS.ITIScodes,
       text   IA5String  (SIZE(1..16))
       } -- # UNTAGGED
  }
 
 
 
-- DF_TransmissionAndSpeed (Desc Name) Record 63
TransmissionAndSpeed ::= OCTET STRING (SIZE(2)) 
    -- Bits 14~16 to be made up of the data element
    -- DE_TransmissionState 
    -- Bits 1~13 to be made up of the data element
    -- DE_Speed
 
 
 
-- DF_ValidRegion (Desc Name) Record 64
ValidRegion ::=  SEQUENCE {
   
   direction         HeadingSlice,
                     -- field of view over which this applies,
   extent            Extent OPTIONAL,  
                     -- the spatial distance over which this
                     -- message applies and should be presented 
                     -- to the driver
   area    CHOICE  {
      shapePointSet  ShapePointSet,
                     -- A short road segment
      circle         Circle,
                     -- A point and radius
      regionPointSet RegionPointSet 
                     -- Wide area enclosed regions
      }
   }
 
 
 
-- DF_VehicleComputedLane (Desc Name) Record 65
VehicleComputedLane ::=  SEQUENCE {
   laneNumber       LaneNumber, 
   laneWidth        LaneWidth  OPTIONAL, 
   laneAttributes   VehicleLaneAttributes  OPTIONAL, 
                    -- if not present, same as ref lane 
   refLaneNum       LaneNumber, 
                    -- number of the ref lane to be used
   lineOffset       DrivenLineOffset, 
   keepOutList      NodeList OPTIONAL,  
                    -- no stop points along the path 
   connectsTo       ConnectsTo  OPTIONAL, 
                    -- a list of other lanes and their
                    -- turning use by this lane
   ... 
   }
 
 
 
-- DF_VehicleIdent (Desc Name) Record 66
VehicleIdent ::=  SEQUENCE {
   name           DescriptiveName OPTIONAL,
                  -- a human readable name for debugging use
   vin            VINstring OPTIONAL,
                  -- vehicle VIN value
   ownerCode      IA5String(SIZE(1..32)) OPTIONAL,
                  -- vehicle owner code 
   id             TemporaryID OPTIONAL, 
                  -- same value used in the BSM
      
   vehicleType    VehicleType  OPTIONAL, 
   vehicleClass   CHOICE 
                  {
                  vGroup ITIS.VehicleGroupAffected,
                  rGroup ITIS.ResponderGroupAffected,
                  rEquip ITIS.IncidentResponseEquipment  
                  } OPTIONAL, 
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_VehicleReferenceLane (Desc Name) Record 67
VehicleReferenceLane ::=  SEQUENCE {
   laneNumber       LaneNumber, 
   laneWidth        LaneWidth  OPTIONAL, 
   laneAttributes   VehicleLaneAttributes, 
   nodeList         NodeList,
                    -- path details of the lane and width
   keepOutList      NodeList OPTIONAL, 
                    -- no stop points along the path 
   connectsTo       ConnectsTo  OPTIONAL, 
                    -- a list of other lanes and their
                    -- turning use by this lane
   ... 
   }
 
 
 
-- DF_VehicleSafetyExtension (Desc Name) Record 68
VehicleSafetyExtension ::=  SEQUENCE {
   events             EventFlags     OPTIONAL,
   pathHistory        PathHistory    OPTIONAL, 
   pathPrediction     PathPrediction OPTIONAL,
   theRTCM            RTCMPackage    OPTIONAL,
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DF_VehicleSize (Desc Name) Record 69
VehicleSize ::=  SEQUENCE {
   width     VehicleWidth,
   length    VehicleLength
   }  -- 3 bytes in length
 
 
 
-- DF_VehicleStatus (Desc Name) Record 70
VehicleStatus ::=  SEQUENCE { 
   lights          ExteriorLights OPTIONAL,                 -- Exterior Lights
   lightBar        LightbarInUse  OPTIONAL,                 -- PS Lights
   
   wipers   SEQUENCE {
         statusFront    WiperStatusFront,
         rateFront      WiperRate,
         statusRear     WiperStatusRear       OPTIONAL,
         rateRear       WiperRate             OPTIONAL
         } OPTIONAL,                                        -- Wipers
   
   brakeStatus  BrakeSystemStatus OPTIONAL, 
                   -- 2 bytes with the following in it:
                   --   wheelBrakes        BrakeAppliedStatus,
                   --                      -x- 4 bits
                   --   traction           TractionControlState,
                   --                      -x- 2 bits
                   --   abs                AntiLockBrakeStatus, 
                   --                      -x- 2 bits
                   --   scs                StablityControlStatus, 
                   --                      -x- 2 bits
                   --   brakeBoost         BrakeBoostApplied, 
                   --                      -x- 2 bits
                   --   spareBits
                   --                      -x- 4 bits
                   -- Note that is present in BSM Part I
                                                            -- Braking Data
   brakePressure   BrakeAppliedPressure  OPTIONAL,          -- Braking Pressure
   roadFriction    CoefficientOfFriction OPTIONAL,          -- Roadway Friction 
   
      
   sunData         SunSensor             OPTIONAL,          -- Sun Sensor        
   rainData        RainSensor            OPTIONAL,          -- Rain Sensor        
   airTemp         AmbientAirTemperature OPTIONAL,          -- Air Temperature    
   airPres         AmbientAirPressure    OPTIONAL,          -- Air Pressure
   
   steering   SEQUENCE {
         angle      SteeringWheelAngle,   
         confidence SteeringWheelAngleConfidence   OPTIONAL,    
         rate       SteeringWheelAngleRateOfChange OPTIONAL,   
         wheels     DrivingWheelAngle              OPTIONAL
         } OPTIONAL,                                        -- steering data
   
   accelSets  SEQUENCE {
         accel4way       AccelerationSet4Way           OPTIONAL, 
         vertAccelThres  VerticalAccelerationThreshold OPTIONAL, 
                                                       -- Wheel Exceeded point
         yawRateCon      YawRateConfidence             OPTIONAL, 
                                                        -- Yaw Rate Confidence
         hozAccelCon     AccelerationConfidence        OPTIONAL,     
                                                        -- Acceleration Confidence 
         confidenceSet   ConfidenceSet                 OPTIONAL
                                                       -- general ConfidenceSet 
         } OPTIONAL, 
                                  
   
   object     SEQUENCE {
         obDist          ObstacleDistance,          -- Obstacle Distance        
         obDirect        ObstacleDirection,         -- Obstacle Direction        
         dateTime        DDateTime                  -- time detected
         } OPTIONAL,                                        -- detected Obstacle data
   
   
   
   fullPos         FullPositionVector OPTIONAL,     -- complete set of time and
                                                    -- position, speed, heading
   throttlePos     ThrottlePosition OPTIONAL,
   speedHeadC      SpeedandHeadingandThrottleConfidence OPTIONAL,    
   speedC          SpeedConfidence OPTIONAL,
   
   vehicleData   SEQUENCE {    
         height        VehicleHeight,
         bumpers       BumperHeights,
         mass          VehicleMass,
         trailerWeight TrailerWeight,
         type          VehicleType 
         -- values for width and length are sent in BSM part I as well. 
         } OPTIONAL,                                        -- vehicle data
   
   vehicleIdent   VehicleIdent OPTIONAL,                    -- comm vehicle data
 
   j1939data      J1939data OPTIONAL,           -- Various SAE J1938 data items
     
   weatherReport SEQUENCE {    
         isRaining        NTCIP.EssPrecipYesNo,
         rainRate         NTCIP.EssPrecipRate       OPTIONAL,
         precipSituation  NTCIP.EssPrecipSituation  OPTIONAL,
         solarRadiation   NTCIP.EssSolarRadiation   OPTIONAL,
         friction         NTCIP.EssMobileFriction   OPTIONAL
         } OPTIONAL,                                        -- local weather data
      
   gpsStatus      GPSstatus          OPTIONAL,              -- vehicle's GPS
 
   ... -- # LOCAL_CONTENT OPTIONAL,        
   }
 
 
 
-- DF_VehicleStatusRequest (Desc Name) Record 71
VehicleStatusRequest ::=  SEQUENCE {
   dataType             VehicleStatusDeviceTypeTag, 
   subType              INTEGER (1..15)   OPTIONAL, 
   sendOnLessThenValue  INTEGER (-32767..32767) OPTIONAL, 
   sendOnMoreThenValue  INTEGER (-32767..32767) OPTIONAL, 
   sendAll              BOOLEAN OPTIONAL, 
   ... 
   }
 
 
 
-- DF_WiperStatus (Desc Name) Record 72
WiperStatus ::=  SEQUENCE {
   statusFront    WiperStatusFront,
   rateFront      WiperRate,
   statusRear     WiperStatusRear      OPTIONAL,
   rateRear       WiperRate            OPTIONAL
   }
 
 
 
-- DF_ITIS_Phrase_WorkZone (Desc Name) Record 73
WorkZone ::=  SEQUENCE (SIZE(1..10)) OF SEQUENCE {
  item CHOICE    {
       itis   ITIS.ITIScodes,
       text   IA5String  (SIZE(1..16))
       } -- # UNTAGGED
  }
 
 
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- Start of entries from table Data_Elements...
-- This table typicaly contains data element entries.
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
 
 
-- DE_Acceleration (Desc Name) Record 1
Acceleration ::= INTEGER (-2000..2001) 
    -- LSB units are 0.01 m/s^2
   -- the value 2000 shall be used for values greater than 2000     
   -- the value -2000 shall be used for values less than -2000  
   -- a value of 2001 shall be used for Unavailable
 
 
 
-- DE_AccelerationConfidence (Desc Name) Record 2
AccelerationConfidence ::= ENUMERATED {
   unavailable  (0), -- B'000  Not Equipped or data is unavailable
   accl-100-00  (1), -- B'001  100  meters / second squared
   accl-010-00  (2), -- B'010  10   meters / second squared
   accl-005-00  (3), -- B'011  5    meters / second squared
   accl-001-00  (4), -- B'100  1    meters / second squared
   accl-000-10  (5), -- B'101  0.1  meters / second squared
   accl-000-05  (6), -- B'110  0.05 meters / second squared
   accl-000-01  (7)  -- B'111  0.01 meters / second squared
   } 
   -- Encoded as a 3 bit value
 
 
 
-- DE_AmbientAirPressure (Barometric Pressure) (Desc Name) Record 3
AmbientAirPressure ::= INTEGER (0..255) 
   -- 8 Bits in hPa starting at 580 with a resolution of 
   -- 2 hPa resulting in a range of 580 to 1090
 
 
 
-- DE_AmbientAirTemperature (Desc Name) Record 4
AmbientAirTemperature ::= INTEGER (0..191) -- in deg C with a -40 offset
 
 
 
-- DE_AntiLockBrakeStatus (Desc Name) Record 5
AntiLockBrakeStatus ::= ENUMERATED {
   unavailable (0), -- B'00  Vehicle Not Equipped with ABS 
                    -- or ABS status is unavailable
   off         (1), -- B'01  Vehicle's ABS is Off
   on          (2), -- B'10  Vehicle's ABS is On (but not engaged)
   engaged     (3)  -- B'11  Vehicle's ABS is Engaged
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_ApproachNumber (Desc Name) Record 6
ApproachNumber ::= INTEGER (0..127)
 
 
 
-- DE_AuxiliaryBrakeStatus (Desc Name) Record 7
AuxiliaryBrakeStatus ::= ENUMERATED {
   unavailable (0), -- B'00  Vehicle Not Equipped with Aux Brakes 
                    -- or Aux Brakes status is unavailable
   off         (1), -- B'01  Vehicle's Aux Brakes are Off
   on          (2), -- B'10  Vehicle's Aux Brakes are On ( Engaged )
   reserved    (3)  -- B'11 
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_J1939-71-Axle Location (Desc Name) Record 8
AxleLocation ::= INTEGER (0..127)
 
 
 
-- DE_J1939-71-Axle Weight (Desc Name) Record 9
AxleWeight ::= INTEGER (0..65535)
 
 
 
-- DE_BarrierAttributes (Desc Name) Record 10
BarrierAttributes ::= INTEGER (0..8192) 
-- With bits as defined:
   noData                   BarrierAttributes ::= 0
                            -- ('0000-0000-0000-0000'B)
   median                   BarrierAttributes ::= 1
                            -- ('0000-0000-0000-0001'B)
   whiteLine                BarrierAttributes ::= 2
                            -- ('0000-0000-0000-0010'B)
   strippedLines            BarrierAttributes ::= 4
                            -- ('0000-0000-0000-0100'B)
   doubleStrippedLines      BarrierAttributes ::= 8
                            -- ('0000-0000-0000-1000'B)
   trafficCones             BarrierAttributes ::= 16
                            -- ('0000-0000-0001-0000'B)
   constructionBarrier      BarrierAttributes ::= 32
                            -- ('0000-0000-0010-0000'B)
   trafficChannels          BarrierAttributes ::= 64
                            -- ('0000-0000-0100-0000'B)
   noCurbs                  BarrierAttributes ::= 128
                            -- ('0000-0000-1000-0000'B)
   lowCurbs                 BarrierAttributes ::= 256
                            -- ('0000-0000-1000-0000'B)
   highCurbs                BarrierAttributes ::= 512
                            -- ('0000-0001-0000-0000'B)
   hovDoNotCross            BarrierAttributes ::= 1024
                            -- ('0000-0010-0000-0000'B)
   hovEntryAllowed          BarrierAttributes ::= 2048
                            -- ('0000-0100-0000-0000'B)
   hovExitAllowed           BarrierAttributes ::= 4096
                            -- ('0000-1000-0000-0000'B)
 
 
 
-- DE_BrakeAppliedPressure (Desc Name) Record 11
BrakeAppliedPressure ::= ENUMERATED {
   unavailable  (0),  -- B'0000 Not Equipped
                      -- or Brake Pres status is unavailable
   minPressure  (1),  -- B'0001 Minimum Braking Pressure
   bkLvl-2      (2),  -- B'0010
   bkLvl-3      (3),  -- B'0011   
   bkLvl-4      (4),  -- B'0100  
   bkLvl-5      (5),  -- B'0101
   bkLvl-6      (6),  -- B'0110
   bkLvl-7      (7),  -- B'0111
   bkLvl-8      (8),  -- B'1000
   bkLvl-9      (9),  -- B'1001
   bkLvl-10     (10), -- B'1010
   bkLvl-11     (11), -- B'1011
   bkLvl-12     (12), -- B'1100
   bkLvl-13     (13), -- B'1101
   bkLvl-14     (14), -- B'1110
   maxPressure  (15)  -- B'1111 Maximum Braking Pressure
   } 
   -- Encoded as a 4 bit value
 
 
 
-- DE_BrakeAppliedStatus (Desc Name) Record 12
BrakeAppliedStatus ::= BIT STRING {
   allOff      (0), -- B'0000  The condition All Off 
   leftFront   (1), -- B'0001  Left Front Active
   leftRear    (2), -- B'0010  Left Rear Active
   rightFront  (4), -- B'0100  Right Front Active
   rightRear   (8)  -- B'1000  Right Rear Active
   } -- to fit in 4 bits
 
 
 
-- DE_BrakeBoostApplied (Desc Name) Record 13
BrakeBoostApplied ::= ENUMERATED {
   unavailable   (0), -- Vehicle not equipped with brake boost
                      -- or brake boost data is unavailable
   off           (1), -- Vehicle's brake boost is off
   on            (2)  -- Vehicle's brake boost is on (applied)
   }
   -- Encoded as a 2 bit value
 
 
 
-- DE_BumperHeightFront (Desc Name) Record 14
BumperHeightFront ::= INTEGER (0..127) -- in units of 0.01 meters from ground surface.
 
 
 
-- DE_BumperHeightRear (Desc Name) Record 15
BumperHeightRear ::= INTEGER (0..127) -- in units of 0.01 meters from ground surface.
 
 
 
-- DE_J1939-71-Cargo Weight (Desc Name) Record 16
CargoWeight ::= INTEGER (0..65535)
 
 
 
-- DE_CodeWord (Desc Name) Record 17
CodeWord ::= OCTET STRING (SIZE(1..16)) 
   -- any octect string up to 16 bytes
 
 
 
-- DE_CoefficientOfFriction (Desc Name) Record 18
CoefficientOfFriction ::= INTEGER (0..50) 
   -- where 0 = 0.00 micro (frictonless)
   -- and 50 = 0.98 micro, in steps of 0.02
 
 
 
-- DE_ColorState (Desc Name) Record 19
ColorState ::= ENUMERATED {
   dark            (0),  -- (B0000) Dark, lights inactive 
   green           (1),  -- (B0001)  
   green-flashing  (9),  -- (B1001)  
   
   yellow          (2),  -- (B0010)  
   yellow-flashing (10), -- (B1010)  
   
   red             (4),  -- (B0100)  
   red-flashing    (12)  -- (B1100)  
   
   } -- a 4 bit encoded value
     -- note that above may be combined 
     -- to create additonal patterns
 
 
 
-- DE_Count (Desc Name) Record 20
Count ::= INTEGER (0..32)
 
 
 
-- DE_CrosswalkLaneAttributes (Desc Name) Record 21
CrosswalkLaneAttributes ::= ENUMERATED {
   noData                     (0),   -- ('0000000000000000'B)
   twoWayPath                 (1),   -- ('0000000000000001'B)
   pedestrianCrosswalk        (2),   -- ('0000000000000010'B)
   bikeLane                   (4),   -- ('0000000000000100'B)
   railRoadTrackPresent       (8),   -- ('0000000000001000'B)
   oneWayPathOfTravel         (16),  -- ('0000000000010000'B)
   pedestrianCrosswalkTypeA   (32),  -- ('0000000000100000'B)
   pedestrianCrosswalkTypeB   (64),  -- ('0000000001000000'B)
   pedestrianCrosswalkTypeC   (128)  -- ('0000000010000000'B)
   }  
   -- MUTCD provides no real "types" to use here
 
 
 
-- DE_DDay (Desc Name) Record 22
DDay ::= INTEGER (0..31)  -- units of days
 
 
 
-- DE_DescriptiveName (Desc Name) Record 23
DescriptiveName ::= IA5String (SIZE(1..63))
 
 
 
-- DE_DHour (Desc Name) Record 24
DHour ::= INTEGER (0..31) -- units of hours
 
 
 
-- DE_DirectionOfUse (Desc Name) Record 25
DirectionOfUse ::= ENUMERATED {
   forward    (0), -- direction of travel follows node ordering 
   reverse    (1), -- direction of travel is the reverse of node ordering 
   both       (2), -- direction of travel allowed in both directions 
   ...
   }
 
 
 
-- DE_DMinute (Desc Name) Record 26
DMinute ::= INTEGER (0..63) -- units of minutes
 
 
 
-- DE_DMonth (Desc Name) Record 27
DMonth ::= INTEGER (0..15) -- units of months
 
 
 
-- DE_DOffset (Desc Name) Record 28
DOffset ::= INTEGER (-840..840) -- units of minutes from UTC time
 
 
 
-- DE_J1939-71-Drive Axle Lift Air Pressure (Desc Name) Record 29
DriveAxleLiftAirPressure ::= INTEGER (0..1000)
 
 
 
-- DE_J1939-71-Drive Axle Location (Desc Name) Record 30
DriveAxleLocation ::= INTEGER (0..255)
 
 
 
-- DE_J1939-71-Drive Axle Lube Pressure (Desc Name) Record 31
DriveAxleLubePressure ::= INTEGER (0..1000)
 
 
 
-- DE_J1939-71-Drive Axle Temperature (Desc Name) Record 32
DriveAxleTemperature ::= INTEGER (-40..210)
 
 
 
-- DE_DrivenLineOffset (Desc Name) Record 33
DrivenLineOffset ::= INTEGER (-32767..32767) 
   -- LSB units are 1 cm.
 
 
 
-- DE_DrivingWheelAngle (Desc Name) Record 34
DrivingWheelAngle ::= INTEGER (-127..127) 
   -- LSB units of 0.3333 degrees.  
   -- a range of 42.33 degrees each way
 
 
 
-- DE_DSecond (Desc Name) Record 35
DSecond ::= INTEGER (0..65535) -- units of miliseconds
 
 
 
-- DE_DSignalSeconds (Desc Name) Record 36
DSignalSeconds ::= INTEGER (0..30000) -- units of 0.01 seconds
 
 
 
-- DE_DSRC_MessageID (Desc Name) Record 37
DSRCmsgID ::= ENUMERATED {
   reserved                        (0), 
   alaCarteMessage                 (1),  -- ACM
   basicSafetyMessage              (2),  -- BSM, heartbeat msg
   basicSafetyMessageVerbose       (3),  -- used for testing only
   commonSafetyRequest             (4),  -- CSR
   emergencyVehicleAlert           (5),  -- EVA
   intersectionCollisionAlert      (6),  -- ICA
   mapData                         (7),  -- MAP, GID, intersections 
   nmeaCorrections                 (8),  -- NMEA
   probeDataManagement             (9),  -- PDM
   probeVehicleData                (10), -- PVD
   roadSideAlert                   (11), -- RSA
   rtcmCorrections                 (12), -- RTCM
   signalPhaseAndTimingMessage     (13), -- SPAT
   signalRequestMessage            (14), -- SRM
   signalStatusMessage             (15), -- SSM
   travelerInformation             (16), -- TIM
   
   ... -- # LOCAL_CONTENT
   } 
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_DYear (Desc Name) Record 38
DYear ::= INTEGER (0..9999) -- units of years
 
 
 
-- DE_Elevation (Desc Name) Record 39
Elevation ::= OCTET STRING (SIZE(2))
   -- 1 decimeter LSB (10 cm) 
   -- Encode elevations from 0 to 6143.9 meters 
   -- above the reference ellipsoid as 0x0000 to 0xEFFF.  
   -- Encode elevations from -409.5 to -0.1 meters, 
   -- i.e. below the reference ellipsoid, as 0xF001 to 0xFFFF
   -- unknown as 0xF000
 
 
 
-- DE_ElevationConfidence (Desc Name) Record 40
ElevationConfidence ::= ENUMERATED {
   unavailable (0),  -- B'0000  Not Equipped or unavailable
   elev-500-00 (1),  -- B'0001  (500 m)
   elev-200-00 (2),  -- B'0010  (200 m)
   elev-100-00 (3),  -- B'0011  (100 m)
   elev-050-00 (4),  -- B'0100  (50 m)
   elev-020-00 (5),  -- B'0101  (20 m)
   elev-010-00 (6),  -- B'0110  (10 m)
   elev-005-00 (7),  -- B'0111  (5 m)
   elev-002-00 (8),  -- B'1000  (2 m)
   elev-001-00 (9),  -- B'1001  (1 m)
   elev-000-50 (10), -- B'1010  (50 cm)
   elev-000-20 (11), -- B'1011  (20 cm)
   elev-000-10 (12), -- B'1100  (10 cm)
   elev-000-05 (13), -- B'1101  (5 cm)
   elev-000-02 (14), -- B'1110  (2 cm)
   elev-000-01 (15)  -- B'1111  (1 cm)
   } 
   -- Encoded as a 4 bit value
 
 
 
-- DE_EmergencyDetails (Desc Name) Record 41
EmergencyDetails ::= INTEGER (0..63) 
   -- First two bit (MSB set to zero.
   -- Combining these 3 items in the remaning 6 bits
   -- sirenUse        SirenInUse                 
   -- lightsUse       LightbarInUse              
   -- multi           MultiVehicleReponse
 
 
 
-- DE_EventFlags (Desc Name) Record 42
EventFlags ::= INTEGER (0..8192) 
-- With bits as defined:
   eventHazardLights               EventFlags ::= 1
   eventStopLineViolation          EventFlags ::= 2 -- Intersection Violation   
   eventABSactivated               EventFlags ::= 4
   eventTractionControlLoss        EventFlags ::= 8
   eventStabilityControlactivated  EventFlags ::= 16
   eventHazardousMaterials         EventFlags ::= 32
   eventEmergencyResponse          EventFlags ::= 64
   eventHardBraking                EventFlags ::= 128
   eventLightsChanged              EventFlags ::= 256
   eventWipersChanged              EventFlags ::= 512
   eventFlatTire                   EventFlags ::= 1024
   eventDisabledVehicle            EventFlags ::= 2048
   eventAirBagDeployment           EventFlags ::= 4096
 
 
 
-- DE_Extent (Desc Name) Record 43
Extent ::= ENUMERATED {
    useInstantlyOnly   (0),
    useFor3meters      (1),
    useFor10meters     (2),
    useFor50meters     (3),
    useFor100meters    (4),
    useFor500meters    (5),
    useFor1000meters   (6),
    useFor5000meters   (7),
    useFor10000meters  (8),
    useFor50000meters  (9),
    useFor100000meters (10),
    forever            (127)  -- very wide area
    }
    -- encode as a single byte
 
 
 
-- DE_ExteriorLights (Desc Name) Record 44
ExteriorLights ::= INTEGER (0..256) 
-- With bits as defined:
   allLightsOff              ExteriorLights ::= 0  
                             -- B'0000-0000  
   lowBeamHeadlightsOn       ExteriorLights ::= 1  
                             -- B'0000-0001
   highBeamHeadlightsOn      ExteriorLights ::= 2 
                             -- B'0000-0010
   leftTurnSignalOn          ExteriorLights ::= 4  
                             -- B'0000-0100
   rightTurnSignalOn         ExteriorLights ::= 8  
                             -- B'0000-1000
   hazardSignalOn            ExteriorLights ::= 12  
                             -- B'0000-1100
   automaticLightControlOn   ExteriorLights ::= 16  
                             -- B'0001-0000
   daytimeRunningLightsOn    ExteriorLights ::= 32  
                             -- B'0010-0000
   fogLightOn                ExteriorLights ::= 64 
                             -- B'0100-0000
   parkingLightsOn           ExteriorLights ::= 128  
                             -- B'1000-0000
 
 
 
-- DE_FurtherInfoID (Desc Name) Record 45
FurtherInfoID ::= OCTET STRING (SIZE(2))
   -- a link to any other incident 
   -- information data that may be available 
   -- in the normal ATIS incident description 
   -- or other messages
   -- two value bytes in length
 
 
 
-- DE_GPSstatus (Desc Name) Record 46
GPSstatus ::= BIT STRING {
   unavailable               (0), -- Not Equipped or unavailable
   isHealthy                 (1),
   isMonitored               (2),
   baseStationType           (3), -- Set to zero if a moving base station,
                                  -- set to one if it is a fixed base station 
   aPDOPofUnder5             (4), -- A dilution of precision greater then 5
   inViewOfUnder5            (5), -- Less then 5 satellites in view
   localCorrectionsPresent   (6),
   networkCorrectionsPresent (7)
   } -- (SIZE(1))
 
 
 
-- DE_Heading (Desc Name) Record 47
Heading ::= INTEGER (0..28800) 
   -- LSB of 0.0125 degrees
   -- A range of 0 to 359.9875 degrees
 
 
 
-- DE_HeadingConfidence (Desc Name) Record 48
HeadingConfidence ::= ENUMERATED {
   unavailable (0), -- B'000  Not Equipped or unavailable
   prec45deg   (1), -- B'001  45   degrees 
   prec10deg   (2), -- B'010  10   degrees
   prec05deg   (3), -- B'011  5    degrees
   prec01deg   (4), -- B'100  1    degrees
   prec0-1deg  (5), -- B'101  0.1  degrees
   prec0-05deg (6), -- B'110  0.05 degrees
   prec0-01deg (7)  -- B'111  0.01 degrees
   } 
   -- Encoded as a 3 bit value
 
 
 
-- DE_HeadingSlice (Desc Name) Record 49
HeadingSlice ::= OCTET STRING (SIZE(2))  
   -- Each bit 22.5 degree starting from 
   -- North and moving Eastward (clockwise)
   
   --  Define global enums for this entry
   noHeading                HeadingSlice ::= '0000'H
   allHeadings              HeadingSlice ::= 'FFFF'H
   
   from000-0to022-5degrees  HeadingSlice ::= '0001'H
   from022-5to045-0degrees  HeadingSlice ::= '0002'H
   from045-0to067-5degrees  HeadingSlice ::= '0004'H
   from067-5to090-0degrees  HeadingSlice ::= '0008'H
   
   from090-0to112-5degrees  HeadingSlice ::= '0010'H
   from112-5to135-0degrees  HeadingSlice ::= '0020'H
   from135-0to157-5degrees  HeadingSlice ::= '0040'H
   from157-5to180-0degrees  HeadingSlice ::= '0080'H
   
   from180-0to202-5degrees  HeadingSlice ::= '0100'H
   from202-5to225-0degrees  HeadingSlice ::= '0200'H
   from225-0to247-5degrees  HeadingSlice ::= '0400'H
   from247-5to270-0degrees  HeadingSlice ::= '0800'H
   
   from270-0to292-5degrees  HeadingSlice ::= '1000'H
   from292-5to315-0degrees  HeadingSlice ::= '2000'H
   from315-0to337-5degrees  HeadingSlice ::= '4000'H
   from337-5to360-0degrees  HeadingSlice ::= '8000'H
 
 
 
-- DE_IntersectionID (Desc Name) Record 50
IntersectionID ::= OCTET STRING (SIZE(2..4))
   -- note that often only the lower 16 bits of this value
   -- will be sent as the operational region (state etc) will
   -- be known and not sent each time
 
 
 
-- DE_Intersection Status Object (Desc Name) Record 51
IntersectionStatusObject ::= OCTET STRING (SIZE(1)) 
   -- with bits set as follows Bit #:
   -- 0    Manual Control is enabled.  Timing reported is per 
   --      programmed values, etc but person at cabinet can 
   --      manually request that certain intervals are terminated 
   --      early (e.g. green).
   -- 1    Stop Time is activated and all counting/timing has stopped.
   -- 2    Intersection is in Conflict Flash.
   -- 3    Preempt is Active
   -- 4    Transit Signal Priority (TSP) is Active
   -- 5    Reserved
   -- 6    Reserved
   -- 7    Reserved as zero
 
 
 
-- DE_LaneCount (Desc Name) Record 52
LaneCount ::= INTEGER (0..255) -- the number of lanes to follow
 
 
 
-- DE_LaneManeuverCode (Desc Name) Record 53
LaneManeuverCode ::= ENUMERATED {
   unknown          (0),  -- used for N.A. as well
   uTurn            (1),  
   leftTurn         (2),   
   rightTurn        (3),   
   straightAhead    (4),   
   softLeftTurn     (5),   
   softRightTurn    (6),   
   ...
   } 
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_LaneNumber (Desc Name) Record 54
LaneNumber ::= OCTET STRING (SIZE(1))
 
 
 
-- DE_LaneSet (Desc Name) Record 55
LaneSet ::= OCTET STRING (SIZE(1..127)) 
    -- each byte encoded as a: LaneNumber, 
    -- the collection of lanes, by num, 
    -- to which some state data applies
 
 
 
-- DE_LaneWidth (Desc Name) Record 56
LaneWidth ::= INTEGER (0..32767) -- units of 1 cm
 
 
 
-- DE_Latitude (Desc Name) Record 57
Latitude ::= INTEGER (-900000000..900000001)  
   -- LSB = 1/10 micro degree
   -- Providing a range of plus-minus 90 degrees
 
 
 
-- DE_LayerID (Desc Name) Record 58
LayerID ::= INTEGER (0..100)
 
 
 
-- DE_LayerType (Desc Name) Record 59
LayerType ::= ENUMERATED {
     none                 (0), 
     mixedContent         (1), -- two or more of the below types
     generalMapData       (2), 
     intersectionData     (3), 
     curveData            (4), 
     roadwaySectionData   (5), 
     parkingAreaData      (6), 
     sharedLaneData       (7),
     ... -- # LOCAL_CONTENT
     } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_LightbarInUse (Desc Name) Record 60
LightbarInUse ::= ENUMERATED {
     unavailable         (0),  -- Not Equipped or unavailable
     notInUse            (1),  -- none active
     inUse               (2),  
     sirenInUse          (3),   
     yellowCautionLights (4),  
     schooldBusLights    (5),  
     arrowSignsActive    (6),  
     slowMovingVehicle   (7),   
     freqStops           (8),   
     reserved            (9)  -- for future use
     }
 
 
 
-- DE_MAYDAY_Location_quality_code (Desc Name) Record 61
Location-quality ::= ENUMERATED {                        
     loc-qual-bt1m       (0), -- quality better than 1 meter
     loc-qual-bt5m       (1), -- quality better than 5 meters
     loc-qual-bt12m      (2), -- quality better than 12.5 meters
     loc-qual-bt50m      (3), -- quality better than 50 meters
     loc-qual-bt125m     (4), -- quality better than 125 meters
     loc-qual-bt500m     (5), -- quality better than 500 meters
     loc-qual-bt1250m    (6), -- quality better than 1250 meters
     loc-qual-unknown    (7)  -- quality value unknown
     } -- 3 bits, appends with loc-tech to make one octet (0..7)
 
 
 
-- DE_MAYDAY_Location_tech_code (Desc Name) Record 62
Location-tech ::= ENUMERATED {                        
     loc-tech-unknown    (0), -- technology type unknown
     loc-tech-GPS        (1), -- GPS technology only
     loc-tech-DGPS       (2), -- differential GPS (DGPS) technology
     loc-tech-drGPS      (3), -- dead reckoning system w/GPS
     loc-tech-drDGPS     (4), -- dead reckoning system w/DGPS
     loc-tech-dr         (5), -- dead reckoning only
     loc-tech-nav        (6), -- autonomous navigation system on-board
     ...,
     loc-tech-fault      (31) -- feature is not working
     }    -- (0..31) 5 bits, appends with loc-quality to make one octet
 
 
 
-- DE_Longitude (Desc Name) Record 63
Longitude ::= INTEGER (-1800000000..1800000001)  
   -- LSB = 1/10 micro degree
   -- Providing a range of plus-minus 180 degrees
 
 
 
-- DE_MinuteOfTheYear (Desc Name) Record 64
MinuteOfTheYear ::= INTEGER (0..525960)
 
 
 
-- DE_MinutesDuration (Desc Name) Record 65
MinutesDuration ::= INTEGER (0..32000) -- units of minutes
 
 
 
-- DE_MsgCount (Desc Name) Record 66
MsgCount ::= INTEGER (0..127)
 
 
 
-- DE_MsgCRC (Desc Name) Record 67
MsgCRC ::= OCTET STRING (SIZE(2)) -- created with the CRC-CCITT polynomial
 
 
 
-- DE_MultiVehicleResponse (Desc Name) Record 68
MultiVehicleResponse ::= ENUMERATED {
     unavailable   (0), -- Not Equipped or unavailable
     singleVehicle (1),  
     multiVehicle  (2),  
     reserved      (3)  -- for future use
     }
 
 
 
-- DE_MUTCDCode (Desc Name) Record 69
MUTCDCode ::= ENUMERATED {
   none            (0), -- non-MUTCD information 
   regulatory      (1), -- "R" Regulatory signs
   warning         (2), -- "W" warning signs
   maintenance     (3), -- "M" Maintenance and construction 
   motoristService (4), -- Motorist Services
   guide           (5), -- "G" Guide signs
   rec             (6), -- Recreation and Cultural Interest 
   ... -- # LOCAL_CONTENT
   } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_NMEA_MsgType (Desc Name) Record 70
NMEA-MsgType ::= INTEGER (0..32767)
 
 
 
-- DE_NMEA_Payload (Desc Name) Record 71
NMEA-Payload ::= OCTET STRING (SIZE(1..1023))
 
 
 
-- DE_NMEA_Revision (Desc Name) Record 72
NMEA-Revision ::= ENUMERATED {
     unknown       (0), 
     reserved      (1),
     rev1          (10),
     rev2          (20),
     rev3          (30),
     rev4          (40),
     rev5          (50),
     ... -- # LOCAL_CONTENT
     } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_NTCIPVehicleclass, (Desc Name) Record 73
NTCIPVehicleclass ::= OCTET STRING (SIZE(1)) 
   -- With bits set as per NTCIP values
   -- Priority Request Vehicle Class Type 
   -- in the upper nibble
   -- Priority Request Vehicle Class Level 
   -- in the lower nibble
 
 
 
-- DE_ObjectCount (Desc Name) Record 74
ObjectCount ::= INTEGER (0..6000) -- a count of objects
 
 
 
-- DE_ObstacleDirection (Desc Name) Record 75
ObstacleDirection ::= Heading -- Use the header DE for this unless it proves different.
 
 
 
-- DE_ObstacleDistance (Desc Name) Record 76
ObstacleDistance ::= INTEGER (0..32767) -- LSB units of meters
 
 
 
-- DE_Payload (Desc Name) Record 77
Payload ::= OCTET STRING (SIZE(1..64))
 
 
 
-- DE_PayloadData (Desc Name) Record 78
PayloadData ::= OCTET STRING (SIZE(1..2048))
 
 
 
-- DE_PedestrianDetect (Desc Name) Record 79
PedestrianDetect ::= ENUMERATED {
   none       (0), -- (B00000001) 
   maybe      (1), -- (B00000010)
   one        (2), -- (B00000100) 
   some       (3), -- (B00001000) Indicates more than one
   ...
   } -- one byte
 
 
 
-- DE_PedestrianSignalState (Desc Name) Record 80
PedestrianSignalState ::= ENUMERATED {
   unavailable (0), -- Not Equipped or unavailable
   stop        (1), -- (B00000001) do not walk
   caution     (2), -- (B00000010) flashing dont walk sign
   walk        (3), -- (B00000100) walk active
   ...
   } -- one byte
 
 
 
-- DE_PositionConfidence (Desc Name) Record 81
PositionConfidence ::= ENUMERATED {
   unavailable (0),  -- B'0000  Not Equipped or unavailable
   a500m   (1), -- B'0001  500m  or about 5 * 10 ^ -3 decimal degrees
   a200m   (2), -- B'0010  200m  or about 2 * 10 ^ -3 decimal degrees
   a100m   (3), -- B'0011  100m  or about 1 * 10 ^ -3 decimal degrees
   a50m    (4), -- B'0100  50m   or about 5 * 10 ^ -4 decimal degrees 
   a20m    (5), -- B'0101  20m   or about 2 * 10 ^ -4 decimal degrees 
   a10m    (6), -- B'0110  10m   or about 1 * 10 ^ -4 decimal degrees 
   a5m     (7), -- B'0111  5m    or about 5 * 10 ^ -5 decimal degrees 
   a2m     (8), -- B'1000  2m    or about 2 * 10 ^ -5 decimal degrees 
   a1m     (9), -- B'1001  1m    or about 1 * 10 ^ -5 decimal degrees 
   a50cm  (10), -- B'1010  0.50m or about 5 * 10 ^ -6 decimal degrees 
   a20cm  (11), -- B'1011  0.20m or about 2 * 10 ^ -6 decimal degrees 
   a10cm  (12), -- B'1100  0.10m or about 1 * 10 ^ -6 decimal degrees 
   a5cm   (13), -- B'1101  0.05m or about 5 * 10 ^ -7 decimal degrees 
   a2cm   (14), -- B'1110  0.02m or about 2 * 10 ^ -7 decimal degrees 
   a1cm   (15)  -- B'1111  0.01m or about 1 * 10 ^ -7 decimal degrees 
   } 
   -- Encoded as a 4 bit value
 
 
 
-- DE_PreemptState (Desc Name) Record 82
PreemptState ::= ENUMERATED {
   none                      (0), -- No preemption (same as value = 2)
   other                     (1), -- Other
   notActive                 (2), -- Not Active (same as value = 0)
   notActiveWithCall         (3), -- Not Active With Call    
   entryStarted              (4), -- Entry Started    
   trackService              (5), -- Track Service
   dwell                     (6), -- Dwell
   linkActive                (7), -- Link Active    
   existStarted              (8), -- Exit Started    
   maximumPresence           (9), -- Max Presence    
   ackowledgedButOverridden (10), -- Ackowledged but Over-ridden    
   ... -- # LOCAL_CONTENT
   }
   -- To use 4 bits,  
   -- typically packed with other items in a BYTE
 
 
 
-- DE_Priority (Desc Name) Record 83
Priority ::= OCTET STRING (SIZE(1)) 
    -- Follow definition notes on setting these bits
 
 
 
-- DE_PriorityState (Desc Name) Record 84
PriorityState ::= ENUMERATED {
   noneActive         (0), -- No signal priority (same as value = 1)
   none               (1), -- TSP None    
   requested          (2), -- TSP Requested    
   active             (3), -- TSP Active    
   activeButIhibitd   (4), -- TSP Reservice (active but inhibited)    
   seccess            (5), -- TSP Success    
   removed            (6), -- TSP Removed    
   clearFail          (7), -- TSP Clear Fail    
   detectFail         (8), -- TSP Detect Fail    
   detectClear        (9), -- TSP Detect Clear    
   abort             (10), -- TSP Abort (needed to remain on-line)
   delayTiming       (11), -- TSP Delay Timing    
   extendTiming      (12), -- TSP Extend Timing    
   preemptOverride   (13), -- TSP Preempt Over-ride    
   adaptiveOverride  (14), -- TSP Adaptive Over-ride    
   reserved          (15),
   ... -- # LOCAL_CONTENT
   } 
   -- To use 4 bits,  
   -- typically packed with other items in a BYTE
 
 
 
-- DE_ProbeSegmentNumber (Desc Name) Record 85
ProbeSegmentNumber ::= INTEGER (0..32767)  
   -- value determined by local device 
   -- as per standard
 
 
 
-- DE_RainSensor (Desc Name) Record 86
RainSensor ::= ENUMERATED {
     none               (0), 
     lightMist          (1), 
     heavyMist          (2), 
     lightRainOrDrizzle (3),
     rain               (4),
     moderateRain       (5),
     heavyRain          (6),
     heavyDownpour      (7) 
     }
 
 
 
-- DE_RequestedItem (Desc Name) Record 87
RequestedItem ::= ENUMERATED {
   reserved      (0),
   itemA         (1),
        -- consisting of 2 elements: 
        -- lights          ExteriorLights 
        -- lightBar        LightbarInUse 
   
   itemB         (2),
        -- consisting of: 
        -- wipers  a SEQUENCE
   
   itemC         (3),
        -- consisting of: 
        -- brakeStatus  BrakeSystemStatus 

   itemD         (4),
        -- consisting of 2 elements: 
        -- brakePressure   BrakeAppliedPressure   
        -- roadFriction    CoefficientOfFriction   
      
   itemE         (5),
        -- consisting of 4 elements: 
        -- sunData         SunSensor                
        -- rainData        RainSensor                   
        -- airTemp         AmbientAirTemperature     
        -- airPres         AmbientAirPressure     
   
   itemF         (6),
        -- consisting of: 
        -- steering  a SEQUENCE 
    
   itemG         (7),
        -- consisting of: 
        -- accelSets a SEQUENCE 
   
   itemH         (8),
        -- consisting of: 
        -- object a SEQUENCE 
   
   
   itemI         (9),
        -- consisting of: 
        -- fullPos         FullPositionVector       
   
   itemJ         (10),
        -- consisting of: 
        -- position2D      Position2D  

   itemK         (11),
        -- consisting of: 
        -- position3D      Position3D 
 
   itemL         (12),
        -- consisting of 2 elements: 
        -- speedHeadC      SpeedandHeadingConfidence     
        -- speedC          SpeedConfidence  
   
   itemM         (13),
        -- consisting of: 
        -- vehicleData a SEQUENCE   
   
   itemN         (14),
        -- consisting of: 
        -- vehicleIdent   VehicleIdent 
   
   itemO         (15),
        -- consisting of: 
        -- weatherReport a SEQUENCE   
   
   itemP         (16),
        -- consisting of: 
        -- breadcrumbs    VehicleMotionTrail 
   
   itemQ         (17),
        -- consisting of: 
        -- gpsStatus      GPSstatus      
      
   ... -- # LOCAL_CONTENT OPTIONAL,        
   }
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_ResponseType (Desc Name) Record 88
ResponseType ::= ENUMERATED {
   notInUseOrNotEquipped      (0),            
   emergency                  (1),            
   nonEmergency               (2),            
   pursuit                    (3)        
   -- all others Future Use
   } 
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_RTCM_ID (Desc Name) Record 89
RTCM-ID ::= INTEGER (0..32767)
 
 
 
-- DE_RTCM_Payload (Desc Name) Record 90
RTCM-Payload ::= OCTET STRING (SIZE(1..1023))
 
 
 
-- DE_RTCM_Revision (Desc Name) Record 91
RTCM-Revision ::= ENUMERATED {
     unknown       (0), 
     reserved      (1),
     rtcmCMR       (2),
     rtcmCMR-Plus  (3),
     rtcmSAPOS     (4),
     rtcmSAPOS-Adv (5),
     rtcmRTCA      (6),
     rtcmRAW       (7),
     rtcmRINEX     (8),
     rtcmSP3       (9),
     rtcmBINEX     (10),
     rtcmRev2-x    (19), -- Used when specific rev is not known
     rtcmRev2-0    (20),
     rtcmRev2-1    (21),
     rtcmRev2-3    (23), -- Std 10402.3
     rtcmRev3-0    (30), 
     rtcmRev3-1    (31), -- Std 10403.1
     ... -- # LOCAL_CONTENT
     } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_SignalLightState (Desc Name) Record 92
SignalLightState ::= INTEGER (0..536870912)
  -- The above bit ranges map to each type of direction
  -- using the bits defined by the above table of the standard.
 
 
 
-- DE_SignalReqScheme (Desc Name) Record 93
SignalReqScheme ::= OCTET STRING (SIZE(1)) 
    -- Encoded as follows: 
   -- upper nibble:  Preempt #:  
    -- Bit 7 (MSB) 1 =  Preempt and 0 =  Priority 
    -- Remaining 3 bits: 
    -- Range of 0..7. The values of 1..6 represent 
    -- the respective controller preempt or Priority 
    -- to be activated. The value of 7 represents a 
    -- request for a cabinet flash prempt, 
    -- while the value of 0 is reserved.  
   
   -- lower nibble:  Strategy #:  
    -- Range is 0..15 and is used to specify a desired 
    -- strategy (if available).  
    -- Currently no strategies are defined and this 
    -- should be zero.
 
 
 
-- DE_SignalState (Desc Name) Record 94
SignalState ::= OCTET STRING (SIZE(1)) 
    -- With bits set as follows:
   
    -- Bit 7 (MSB) Set if the state is currently active
    -- only one active state can exist at a time, and 
    -- this state should be sent first in any sequences
   
    -- Bits 6~4 The preempt or priority value that is 
    -- being described.  
   
    -- Bits 3~0 the state bits, indicating either a 
    -- preemption or a priority use as follows: 
   
    -- If a preemption: to follow the 
    -- preemptState object of NTCIP 1202 v2.19f  
    -- See PreemptState for bit definitions.
   
    -- If a prioirty to follow the 
    -- tspInputStatus object utilized in the 
    -- NYC ASTC2 traffic controller
    -- See PriorityState for bit definitions
 
 
 
-- DE_SignPrority (Desc Name) Record 95
SignPrority ::= INTEGER (0..7)
   -- 0 as least, 7 as most
 
 
 
-- DE_SirenInUse (Desc Name) Record 96
SirenInUse ::= ENUMERATED {
     unavailable   (0), -- Not Equipped or unavailable
     notInUse      (1),  
     inUse         (2),  
     reserved      (3)  -- for future use
     }
 
 
 
-- DE_SpecialLaneAttributes (Desc Name) Record 97
SpecialLaneAttributes ::= ENUMERATED {
   noData                    (0), -- ('0000000000000000'B)
   egressPath                (1), -- ('0000000000000001'B)
       -- a two-way path or an outbound path is described 
   railRoadTrack             (2), -- ('0000000000000010'B)
   transitOnlyLane           (4), -- ('0000000000000100'B)
   hovLane                   (8), -- ('0000000000001000'B)
   busOnly                  (16), -- ('0000000000010000'B)
   vehiclesEntering         (32), -- ('0000000000100000'B)
   vehiclesLeaving          (64), -- ('0000000001000000'B)
   reserved                (128)  -- ('0000000010000000'B)
   } -- 1 byte
 
 
 
-- DE_SpecialSignalState (Desc Name) Record 98
SpecialSignalState ::= ENUMERATED {
   unknown    (0),
   notInUse   (1), -- (B0001) default state, empty, not in use
   arriving   (2), -- (B0010) track-lane about to be occupied   
   present    (3), -- (B0100) track-lane is occupied with vehicle 
   departing  (4), -- (B1000) track-lane about to be empty 
   ...
   } -- one byte
 
 
 
-- DE_Speed (Desc Name) Record 99
Speed ::= INTEGER (0..8191) -- Units of 0.02 m/s
          -- The value 8191 indicates that 
          -- speed is unavailable
 
 
 
-- DE_SpeedConfidence (Desc Name) Record 100
SpeedConfidence ::= ENUMERATED {
   unavailable (0), -- B'000  Not Equipped or unavailable
   prec100ms   (1), -- B'001  100  meters / sec
   prec10ms    (2), -- B'010  10   meters / sec
   prec5ms     (3), -- B'011  5    meters / sec
   prec1ms     (4), -- B'100  1    meters / sec
   prec0-1ms   (5), -- B'101  0.1  meters / sec
   prec0-05ms  (6), -- B'110  0.05 meters / sec
   prec0-01ms  (7)  -- B'111  0.01 meters / sec
   } 
   -- Encoded as a 3 bit value
 
 
 
-- DE_StabilityControlStatus (Desc Name) Record 101
StabilityControlStatus ::= ENUMERATED {
   unavailable (0), -- B'00  Not Equipped with SC
                    -- or SC status is unavailable
   off         (1), -- B'01  Off
   on          (2)  -- B'10  On or active (engaged)
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_StateConfidence (Desc Name) Record 102
StateConfidence ::= ENUMERATED {
   unKnownEstimate        (0), 
   minTime                (1), 
   maxTime                (2), 
   timeLikeklyToChange    (3),
   ... -- # LOCAL_CONTENT
   }
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_J1939-71-Steering Axle Lube Pressure (Desc Name) Record 103
SteeringAxleLubePressure ::= INTEGER (0..255)
 
 
 
-- DE_J1939-71-Steering Axle Temperature (Desc Name) Record 104
SteeringAxleTemperature ::= INTEGER (0..255)
 
 
 
-- DE_SteeringWheelAngle (Desc Name) Record 105
SteeringWheelAngle ::= OCTET STRING (SIZE(1)) 
    -- LSB units of 1.5 degrees.  
    -- a range of -189 to +189 degrees
    -- 0x01 = 00 = +1.5 deg
    -- 0x81 = -126 = -189 deg and beyond
    -- 0x7E = +126 = +189 deg and beyond
    -- 0x7F = +127 to be used for unavailable
 
 
 
-- DE_SteeringWheelAngleConfidence (Desc Name) Record 106
SteeringWheelAngleConfidence ::= ENUMERATED {
   unavailable (0), -- B'00  Not Equipped with Wheel angle
                    -- or Wheel angle status is unavailable
   prec2deg    (1), -- B'01  2 degrees
   prec1deg    (2), -- B'10  1 degree
   prec0-02deg (3)  -- B'11  0.02 degrees
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_SteeringWheelAngleRateOfChange (Desc Name) Record 107
SteeringWheelAngleRateOfChange ::= INTEGER (-127..127) 
   -- LSB is 3 degrees per second
 
 
 
-- DE_SunSensor (Desc Name) Record 108
SunSensor ::= INTEGER (0..1000) 
   -- units of watts / m2
 
 
 
-- DE_TemporaryID (Desc Name) Record 109
TemporaryID ::= OCTET STRING (SIZE(4)) -- a 4 byte string array
 
 
 
-- DE_TerminationDistance (Desc Name) Record 110
TermDistance ::= INTEGER (1..30000)   -- units in meters
 
 
 
-- DE_TerminationTime (Desc Name) Record 111
TermTime ::= INTEGER (1..1800) -- units of sec
 
 
 
-- DE_ThrottleConfidence (Desc Name) Record 112
ThrottleConfidence ::= ENUMERATED {
   unavailable     (0), -- B'00  Not Equipped or unavailable
   prec10percent   (1), -- B'01  10  percent Confidence level
   prec1percent    (2), -- B'10  1   percent Confidence level
   prec0-5percent  (3)  -- B'11  0.5 percent Confidence level
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_ThrottlePosition (Desc Name) Record 113
ThrottlePosition ::= INTEGER (0..200) -- LSB units are 0.5 percent
 
 
 
-- DE_TimeConfidence (Desc Name) Record 114
TimeConfidence ::= ENUMERATED {
   unavailable              (0), -- Not Equipped or unavailable
   time-100-000             (1), -- Better then  100 Seconds
   time-050-000             (2), -- Better then  50 Seconds
   time-020-000             (3), -- Better then  20 Seconds
   time-010-000             (4), -- Better then  10 Seconds
   time-002-000             (5), -- Better then  2 Seconds
   time-001-000             (6), -- Better then  1 Second
   time-000-500             (7), -- Better then  0.5 Seconds
   time-000-200             (8), -- Better then  0.2 Seconds
   time-000-100             (9), -- Better then  0.1 Seconds
   time-000-050            (10), -- Better then  0.05 Seconds
   time-000-020            (11), -- Better then  0.02 Seconds
   time-000-010            (12), -- Better then  0.01 Seconds
   time-000-005            (13), -- Better then  0.005 Seconds
   time-000-002            (14), -- Better then  0.002 Seconds
   time-000-001            (15), -- Better then  0.001 Seconds
                                 -- Better then one milisecond
   time-000-000-5          (16), -- Better then  0.000,5 Seconds
   time-000-000-2          (17), -- Better then  0.000,2 Seconds
   time-000-000-1          (18), -- Better then  0.000,1 Seconds
   time-000-000-05         (19), -- Better then  0.000,05 Seconds
   time-000-000-02         (20), -- Better then  0.000,02 Seconds
   time-000-000-01         (21), -- Better then  0.000,01 Seconds
   time-000-000-005        (22), -- Better then  0.000,005 Seconds
   time-000-000-002        (23), -- Better then  0.000,002 Seconds
   time-000-000-001        (24), -- Better then  0.000,001 Seconds 
                                 -- Better then one micro second
   time-000-000-000-5      (25), -- Better then  0.000,000,5 Seconds
   time-000-000-000-2      (26), -- Better then  0.000,000,2 Seconds
   time-000-000-000-1      (27), -- Better then  0.000,000,1 Seconds
   time-000-000-000-05     (28), -- Better then  0.000,000,05 Seconds
   time-000-000-000-02     (29), -- Better then  0.000,000,02 Seconds
   time-000-000-000-01     (30), -- Better then  0.000,000,01 Seconds
   time-000-000-000-005    (31), -- Better then  0.000,000,005 Seconds
   time-000-000-000-002    (32), -- Better then  0.000,000,002 Seconds
   time-000-000-000-001    (33), -- Better then  0.000,000,001 Seconds
                                 -- Better then one nano second
   time-000-000-000-000-5  (34), -- Better then  0.000,000,000,5 Seconds
   time-000-000-000-000-2  (35), -- Better then  0.000,000,000,2 Seconds
   time-000-000-000-000-1  (36), -- Better then  0.000,000,000,1 Seconds
   time-000-000-000-000-05 (37), -- Better then  0.000,000,000,05 Seconds
   time-000-000-000-000-02 (38), -- Better then  0.000,000,000,02 Seconds
   time-000-000-000-000-01 (39)  -- Better then  0.000,000,000,01 Seconds 
   }
 
 
 
-- DE_TimeMark (Desc Name) Record 115
TimeMark ::= INTEGER (0..12002)
   -- In units of 1/10th second from local UTC time
   -- A range of 0~600 for even minutes, 601~1200 for odd minutes
   -- 12001 to indicate indefinite time
   -- 12002 to be used when value undefined or unknown
 
 
 
-- DE_J1939-71-Tire Leakage Rate (Desc Name) Record 116
TireLeakageRate ::= INTEGER (0..65535)
 
 
 
-- DE_J1939-71-Tire Location (Desc Name) Record 117
TireLocation ::= INTEGER (0..255)
 
 
 
-- DE_J1939-71-Tire Pressure (Desc Name) Record 118
TirePressure ::= INTEGER (0..1000)
 
 
 
-- DE_J1939-71-Tire Pressure Threshold Detection (Desc Name) Record 119
TirePressureThresholdDetection ::= ENUMERATED {
   noData                (0),  -- B'000'
   overPressure          (1),  -- B'001' 
   noWarningPressure     (2),  -- B'010'
   underPressure         (3),  -- B'011'
   extremeUnderPressure  (4),  -- B'100'
   undefined             (5),  -- B'101'
   errorIndicator        (6),  -- B'110'
   notAvailable          (7),  -- B'111'
   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DE_J1939-71-Tire Temp (Desc Name) Record 120
TireTemp ::= INTEGER (0..65535)
 
 
 
-- DE_TractionControlState (Desc Name) Record 121
TractionControlState ::= ENUMERATED {
   unavailable (0), -- B'00  Not Equipped with tracton control 
                    -- or tracton control status is unavailable
   off         (1), -- B'01  tracton control is Off
   on          (2), -- B'10  tracton control is On (but not Engaged)
   engaged     (3)  -- B'11  tracton control is Engaged
   } 
   -- Encoded as a 2 bit value
 
 
 
-- DE_J1939-71-Trailer Weight (Desc Name) Record 122
TrailerWeight ::= INTEGER (0..65535)
 
 
 
-- DE_TransitPreEmptionRequest (Desc Name) Record 123
TransitPreEmptionRequest ::= ENUMERATED {
     typeOne       (0), 
     typeTwo       (1), 
     typeThree     (2), 
     typeFour      (3),
     ... -- # LOCAL_CONTENT
     } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_TransitStatus (Desc Name) Record 124
TransitStatus ::= BIT STRING {
    none        (0), -- nothing is active
    anADAuse    (1), -- an ADA access is in progress (wheelchairs, kneling, etc.)
    aBikeLoad   (2), -- loading of a bicyle is in progress
    doorOpen    (3), -- a vehicle door is open for passenger access
    occM        (4),
    occL        (5)
    -- bits four and five are used to relate the 
    -- the relative occupancy of the vehicle, with
    -- 00  as least full and 11 indicating a 
    -- close-to or full conditon
    } (SIZE(6))
 
 
 
-- DE_TransmissionState (Desc Name) Record 125
TransmissionState ::= ENUMERATED {
   neutral         (0), -- Neutral, speed relative to the vehicle alignment
   park            (1), -- Park, speed relative the to vehicle alignment
   forwardGears    (2), -- Forward gears, speed relative the to vehicle alignment
   reverseGears    (3), -- Reverse gears, speed relative the to vehicle alignment 
   reserved1       (4),      
   reserved2       (5),      
   reserved3       (6),      
   unavailable     (7), -- not-equipped or unavailable value,
                        -- speed relative to the vehicle alignment

   ... -- # LOCAL_CONTENT
   }
 
 
 
-- DE_TravelerInfoType (Desc Name) Record 126
TravelerInfoType ::= ENUMERATED {
     unknown            (0), 
     advisory           (1),
     roadSignage        (2), 
     commercialSignage  (3),
     ... -- # LOCAL_CONTENT
     } 
     -- values to 127 reserved for std use
     -- values 128 to 255 reserved for local use
 
 
 
-- DE_TransmitInterval (Desc Name) Record 127
TxTime ::= INTEGER (1..20)   -- units of seconds
 
 
 
-- DE_UniqueMSG_ID (Desc Name) Record 128
UniqueMSGID ::= OCTET STRING (SIZE(9))
 
 
 
-- DE_URL_Base (Desc Name) Record 129
URL-Base ::= IA5String (SIZE(1..45))
 
 
 
-- DE_URL_Link (Desc Name) Record 130
URL-Link ::= IA5String (SIZE(1..255))
 
 
 
-- DE_URL_Short (Desc Name) Record 131
URL-Short ::= IA5String (SIZE(1..15))
 
 
 
-- DE_VehicleHeight (Desc Name) Record 132
VehicleHeight ::= INTEGER (0..127) 
    -- the height of the vehicle
    -- LSB units of 5 cm, range to 6.35 meters
 
 
 
-- DE_VehicleLaneAttributes (Desc Name) Record 133
VehicleLaneAttributes ::= INTEGER (0..65535) 
-- With bits as defined:
    noLaneData              VehicleLaneAttributes ::= 0 
                            -- ('0000000000000000'B)
    egressPath              VehicleLaneAttributes ::= 1 
                            -- ('0000000000000001'B)
                            -- a two-way path or an outbound 
                            -- path is described 
    maneuverStraightAllowed VehicleLaneAttributes ::= 2 
                            -- ('0000000000000010'B)
    maneuverLeftAllowed     VehicleLaneAttributes ::= 4 
                            -- ('0000000000000100'B)
    maneuverRightAllowed    VehicleLaneAttributes ::= 8 
                            -- ('0000000000001000'B)
    yield                   VehicleLaneAttributes ::= 16 
                            -- ('0000000000010000'B)
    maneuverNoUTurn         VehicleLaneAttributes ::= 32 
                            -- ('0000000000100000'B)
    maneuverNoTurnOnRed     VehicleLaneAttributes ::= 64 
                            -- ('0000000001000000'B)
    maneuverNoStop          VehicleLaneAttributes ::= 128 
                            -- ('0000000010000000'B)
    noStop                  VehicleLaneAttributes ::= 256 
                            -- ('0000000100000000'B)
    noTurnOnRed             VehicleLaneAttributes ::= 512 
                            -- ('0000001000000000'B)
    hovLane                 VehicleLaneAttributes ::= 1024 
                            -- ('0000010000000000'B)
    busOnly                 VehicleLaneAttributes ::= 2048 
                            -- ('0000100000000000'B)
    busAndTaxiOnly          VehicleLaneAttributes ::= 4096 
                            -- ('0001000000000000'B)
    maneuverHOVLane         VehicleLaneAttributes ::= 8192 
                            -- ('0010000000000000'B)
    maneuverSharedLane      VehicleLaneAttributes ::= 16384 
                            -- ('0100000000000000'B) 
                            -- a "TWLTL" (two way left turn lane)
    maneuverBikeLane        VehicleLaneAttributes ::= 32768
                            -- ('1000000000000000'B)
 
 
 
-- DE_VehicleLength (Desc Name) Record 134
VehicleLength ::= INTEGER (0..16383) -- LSB units are 1 cm
 
 
 
-- DE_VehicleMass (Desc Name) Record 135
VehicleMass ::= INTEGER (0..127) -- mass with an LSB of 50 Kg
 
 
 
-- DE_VehicleRequestStatus (Desc Name) Record 136
VehicleRequestStatus ::= OCTET STRING (SIZE(1)) 
  -- With bits set as follows: 
  -- Bit 7 (MSB)  Brakes-on, see notes for use 
  -- Bit 6 Emergency Use or operation 
  -- Bit 5 Lights in use (see also the light bar element)
  -- Bits 5~0
  -- when a priority, map the values of 
  -- LightbarInUse to the lower 4 bits 
  -- and set the 5th bit to zero
  -- when a preemption, map the values of 
  -- TransistStatus to the lower 5 bits
 
 
 
-- DE_VehicleStatusDeviceTypeTag (Desc Name) Record 137
VehicleStatusDeviceTypeTag ::= ENUMERATED {
   unknown            (0),
   lights             (1),  -- Exterior Lights
   wipers             (2),  -- Wipers
   brakes             (3),  -- Brake Applied                
   stab               (4),  -- Stability Control        
   trac               (5),  -- Traction Control        
   abs                (6),  -- Anti-Lock Brakes        
   sunS               (7),  -- Sun Sensor        
   rainS              (8),  -- Rain Sensor        
   airTemp            (9),  -- Air Temperature    
   steering           (10),
   vertAccelThres     (11), -- Wheel that Exceeded the
   vertAccel          (12), -- Vertical g Force Value  
   hozAccelLong       (13), -- Longitudinal Acceleration        
   hozAccelLat        (14), -- Lateral Acceleration        
   hozAccelCon        (15), -- Acceleration Confidence 
   accel4way          (16),
   confidenceSet      (17),
   obDist             (18), -- Obstacle Distance        
   obDirect           (19), -- Obstacle Direction        
   yaw                (20), -- Yaw Rate        
   yawRateCon         (21), -- Yaw Rate Confidence
   dateTime           (22), -- complete time
   fullPos            (23), -- complete set of time and
                            -- position, speed, heading
   position2D         (24), -- lat, long
   position3D         (25), -- lat, long, elevation
   vehicle            (26), -- height, mass, type
   speedHeadC         (27), 
   speedC             (28),
   
   ... -- # LOCAL_CONTENT
   }
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_VehicleType (Desc Name) Record 138
VehicleType ::= ENUMERATED {
   none                 (0),  -- Not Equipped, Not known or unavailable
   unknown              (1),  -- Does not fit any other category    
   special              (2),  -- Special use    
   moto                 (3),  -- Motorcycle    
   car                  (4),  -- Passenger car    
   carOther             (5),  -- Four tire single units    
   bus                  (6),  -- Buses    
   axleCnt2             (7),  -- Two axle, six tire single units    
   axleCnt3             (8),  -- Three axle, single units    
   axleCnt4             (9),  -- Four or more axle, single unit    
   axleCnt4Trailer      (10), -- Four or less axle, single trailer    
   axleCnt5Trailer      (11), -- Five or less axle, single trailer    
   axleCnt6Trailer      (12), -- Six or more axle, single trailer    
   axleCnt5MultiTrailer (13), -- Five or less axle, multi-trailer    
   axleCnt6MultiTrailer (14), -- Six axle, multi-trailer    
   axleCnt7MultiTrailer (15),  -- Seven or more axle, multi-trailer    
   ... -- # LOCAL_CONTENT
   } 
   -- values to 127 reserved for std use
   -- values 128 to 255 reserved for local use
 
 
 
-- DE_VehicleWidth (Desc Name) Record 139
VehicleWidth ::= INTEGER (0..1023) -- LSB units are 1 cm
 
 
 
-- DE_VerticalAcceleration (Desc Name) Record 140
VerticalAcceleration ::= INTEGER (-127..127) 
   -- LSB units of 0.02 G steps over 
   -- a range +1.54 to -3.4G 
   -- and offset by 50  Value 50 = 0g, Value 0 = -1G
   -- value +127 = 1.54G, 
   -- value -120 = -3.4G
   -- value -121 for ranges -3.4 to -4.4G
   -- value -122 for ranges -4.4 to -5.4G
   -- value -123 for ranges -5.4 to -6.4G
   -- value -124 for ranges -6.4 to -7.4G
   -- value -125 for ranges -7.4 to -8.4G
   -- value -126 for ranges larger than -8.4G
   -- value -127 for unavailable data
 
 
 
-- DE_VerticalAccelerationThreshold (Desc Name) Record 141
VerticalAccelerationThreshold ::= BIT STRING {
   allOff      (0), -- B'0000  The condition All Off or not equipped
   leftFront   (1), -- B'0001  Left Front Event
   leftRear    (2), -- B'0010  Left Rear Event
   rightFront  (4), -- B'0100  Right Front Event
   rightRear   (8)  -- B'1000  Right Rear Event
   } -- to fit in 4 bits
 
 
 
-- DE_VINstring, (Desc Name) Record 142
VINstring ::= OCTET STRING (SIZE(1..17)) 
   -- A legal VIN or a shorter value 
   -- to provide an ident of the vehicle
   -- If a VIN is sent, then IA5 encoding 
   -- shall be used
 
 
 
-- DE_J1939-71-Wheel End Elect. Fault (Desc Name) Record 143
WheelEndElectFault ::= BIT STRING {
   bitOne      (1),
   bitTwo      (2),
   bitThree    (3),
   bitFour     (4)
   }
 
 
 
-- DE_J1939-71-Wheel Sensor Status (Desc Name) Record 144
WheelSensorStatus ::= ENUMERATED {
     off           (0), 
     on            (1),
     notDefined    (2), 
     notSupoprted  (3)
     }
 
 
 
-- DE_WiperRate (Desc Name) Record 145
WiperRate ::= INTEGER (0..127) -- units of sweeps per minute
 
 
 
-- DE_WiperStatusFront (Desc Name) Record 146
WiperStatusFront ::= ENUMERATED {
     unavailable         (0), -- Not Equipped with wiper status
                              -- or wiper status is unavailable
     off                 (1),  
     intermittent        (2), 
     low                 (3),
     high                (4),
     washerInUse       (126), -- washing solution being used
     automaticPresent  (127), -- Auto wiper equipped
     ... -- # LOCAL_CONTENT
     }
 
 
 
-- DE_WiperStatusRear (Desc Name) Record 147
WiperStatusRear ::= ENUMERATED {
     unavailable         (0), -- Not Equipped with wiper status
                              -- or wiper status is unavailable
     off                 (1),  
     intermittent        (2), 
     low                 (3),
     high                (4),
     washerInUse       (126), -- washing solution being used
     automaticPresent  (127), -- Auto wipper equipped
     ... -- # LOCAL_CONTENT
     }
 
 
 
-- DE_YawRate (Desc Name) Record 148
YawRate ::= INTEGER (-32767..32767) 
   -- LSB units of 0.01 degrees per second (signed)
 
 
 
-- DE_YawRateConfidence (Desc Name) Record 149
YawRateConfidence ::= ENUMERATED {
   unavailable    (0), -- B'000  Not Equipped with yaw rate status
                       -- or yaw rate status is unavailable
   degSec-100-00  (1), -- B'001  100  deg/sec
   degSec-010-00  (2), -- B'010  10   deg/sec
   degSec-005-00  (3), -- B'011  5    deg/sec
   degSec-001-00  (4), -- B'100  1    deg/sec
   degSec-000-10  (5), -- B'101  0.1  deg/sec
   degSec-000-05  (6), -- B'110  0.05 deg/sec
   degSec-000-01  (7)  -- B'111  0.01 deg/sec
   }  
   -- Encoded as a 3 bit value
 
 


-- Unable to find the file: DSRCstubs.txt
-- Which would be be inserted at this point if present.


END 
-- end of the DSRC module.
 
 
 
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- Start of External Data entries...
-- Grouped into sets of modules
-- -_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
-- 
-- ^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-
-- 
-- Begin module: NTCIP
-- 
-- ^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-
NTCIP DEFINITIONS AUTOMATIC TAGS::= BEGIN 
 


-- ESS_EssMobileFriction (Desc Name) Record 1
-- From source: NTCIP 1204
EssMobileFriction ::= INTEGER (0..101)

-- ESS_EssPrecipRate_quantity (Desc Name) Record 2
-- From source: NTCIP 1204
EssPrecipRate ::= INTEGER (0..65535) 

-- ESS_EssPrecipSituation_code (Desc Name) Record 3
-- From source: NTCIP 1204
EssPrecipSituation ::= ENUMERATED {
   other (1), 
   unknown (2), 
   noPrecipitation (3), 
   unidentifiedSlight (4), 
   unidentifiedModerate (5), 
   unidentifiedHeavy (6), 
   snowSlight (7), 
   snowModerate (8), 
   snowHeavy (9), 
   rainSlight (10), 
   rainModerate (11), 
   rainHeavy (12), 
   frozenPrecipitationSlight (13), 
   frozenPrecipitationModerate (14), 
   frozenPrecipitationHeavy (15)
   }

-- ESS_EssPrecipYesNo_code (Desc Name) Record 4
-- From source: NTCIP 1204
EssPrecipYesNo ::= ENUMERATED {precip (1), noPrecip (2), error (3)} 

-- ESS_EssSolarRadiation_quantity (Desc Name) Record 5
-- From source: NTCIP 1204
EssSolarRadiation ::= INTEGER (0..65535) 


-- Inserting file: NTCIPstubs.txt here.


-- This is a collection of code and stubs needed to match to the
-- NTCIP (and ESS) work. 


-- End of inserted file 

 
END
-- End of the NTCIP module.
 
-- ^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-
-- 
-- Begin module: ITIS
-- 
-- ^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-^-
ITIS DEFINITIONS AUTOMATIC TAGS::= BEGIN 
 


-- DE_Incident Response Equipment (Desc Name) Record 6
-- From source: SAE ITIS Terms
IncidentResponseEquipment ::= ENUMERATED { 
   ground-fire-suppression          (9985),   
   heavy-ground-equipment           (9986),   
   aircraft                         (9988),   
   marine-equipment                 (9989),   
   support-equipment                (9990),   
   medical-rescue-unit              (9991),   
   other                            (9993),   -- Depreciated by fire standards, do not
                                              -- use
   ground-fire-suppression-other    (9994),   
   engine                           (9995),   
   truck-or-aerial                  (9996),   
   quint                            (9997),   -- A five-function type of fire apparatus.
                                              -- The units in the movie Backdraft were
                                              -- quints
   tanker-pumper-combination        (9998),   
   brush-truck                      (10000),  
   aircraft-rescue-firefighting     (10001),  
   heavy-ground-equipment-other     (10004),  
   dozer-or-plow                    (10005),  
   tractor                          (10006),  
   tanker-or-tender                 (10008),  
   aircraft-other                   (10024),  
   aircraft-fixed-wing-tanker       (10025),  
   helitanker                       (10026),  
   helicopter                       (10027),  
   marine-equipment-other           (10034),  
   fire-boat-with-pump              (10035),  
   boat-no-pump                     (10036),  
   support-apparatus-other          (10044),  
   breathing-apparatus-support      (10045),  
   light-and-air-unit               (10046),  
   medical-rescue-unit-other        (10054),  
   rescue-unit                      (10055),  
   urban-search-rescue-unit         (10056),  
   high-angle-rescue                (10057),  
   crash-fire-rescue                (10058),  
   bLS-unit                         (10059),  
   aLS-unit                         (10060),  
   mobile-command-post              (10075),  -- Depreciated, do not use
   chief-officer-car                (10076),  
   hAZMAT-unit                      (10077),  
   type-i-hand-crew                 (10078),  
   type-ii-hand-crew                (10079),  
   privately-owned-vehicle          (10083),  -- (Often found in volunteer fire teams)
   other-apparatus-resource         (10084),  -- (Remapped from fire code zero)
   ambulance                        (10085),  
   bomb-squad-van                   (10086),  
   combine-harvester                (10087),  
   construction-vehicle             (10088),  
   farm-tractor                     (10089),  
   grass-cutting-machines           (10090),  
   hAZMAT-containment-tow           (10091),  
   heavy-tow                        (10092),  
   light-tow                        (10094),  
   flatbed-tow                      (10114), 
   hedge-cutting-machines           (10093),  
   mobile-crane                     (10095),  
   refuse-collection-vehicle        (10096),  
   resurfacing-vehicle              (10097),  
   road-sweeper                     (10098),  
   roadside-litter-collection-crews (10099),  
   salvage-vehicle                  (10100),  
   sand-truck                       (10101),  
   snowplow                         (10102),  
   steam-roller                     (10103),  
   swat-team-van                    (10104),  
   track-laying-vehicle             (10105),  
   unknown-vehicle                  (10106),  
   white-lining-vehicle             (10107),  -- Consider using Roadwork "road marking
                                              -- operations" unless the objective is to
                                              -- refer to the specific vehicle of this
                                              -- type.  Alternative Rendering: line
                                              -- painting vehicle
   dump-truck                       (10108),  
   supervisor-vehicle               (10109),  
   snow-blower                      (10110),  
   rotary-snow-blower               (10111),  
   road-grader                      (10112),  -- Alternative term: motor grader
   steam-truck                      (10113),  -- A special truck that thaws culverts and
                                              -- storm drains
   ... -- # LOCAL_CONTENT_ITIS 
   }

-- EXT_ITIS_Codes (Desc Name) Record 7
-- From source: Adopted SAE J2540-2  (ITIS Phrases), March 2002
ITIScodes ::= INTEGER (0..65565)
-- The defined list of ITIS codes is too long to list here
-- Many smaller lists use a sub-set of these codes as defined elements
-- Also enumerated values expressed as text constant are very common,
-- and in many deployments the list codes are used as a shorthand for
-- this text.  Also the XML expressions commonly use a union of the 
-- code values and the textual expressions.  
-- Consult SAE J2540 for further details.

-- DF_ITIS-Codes_And_Text (Desc Name) Record 8
-- From source: Adopted SAE J2540-2  (ITIS Phrases), March 2002
ITIScodesAndText ::=  SEQUENCE (SIZE(1..100)) OF SEQUENCE {
  item CHOICE    {
       itis ITIScodes,
       text ITIStext
       } -- # UNTAGGED
  }

-- DE_ITIS_Text (Desc Name) Record 9
-- From source: Adopted SAE J2540-2  (ITIS Phrases), March 2002
ITIStext ::= IA5String (SIZE(1..500))

-- DE_Responder Group Affected (Desc Name) Record 10
-- From source: SAE ITIS Terms
ResponderGroupAffected ::= ENUMERATED  { 
   emergency-vehicle-units           (9729),  -- Default phrase, to be used when one of
                                              -- the below does not fit better
   federal-law-enforcement-units     (9730),  
   state-police-units                (9731),  
   county-police-units               (9732),  -- Hint: also sheriff response units
   local-police-units                (9733),  
   ambulance-units                   (9734),  
   rescue-units                      (9735),  
   fire-units                        (9736),  
   hAZMAT-units                      (9737),  
   light-tow-unit                    (9738),  
   heavy-tow-unit                    (9739),  
   freeway-service-patrols           (9740),  
   transportation-response-units     (9741),  
   private-contractor-response-units (9742),  
   ... -- # LOCAL_CONTENT_ITIS 
   }
   -- These groups are used in coordinated response and staging area information
   -- (rather than typically consumer related)


-- DE_Vehicle Groups Affected (Desc Name) Record 11
-- From source: SAE ITIS Terms
VehicleGroupAffected ::= ENUMERATED  { 
   all-vehicles                               (9217),  
   bicycles                                   (9218),  
   motorcycles                                (9219),  -- to include mopeds as well
   cars                                       (9220),  -- (remapped from ERM value of
                                                       -- zero)
   light-vehicles                             (9221),  
   cars-and-light-vehicles                    (9222),  
   cars-with-trailers                         (9223),  
   cars-with-recreational-trailers            (9224),  
   vehicles-with-trailers                     (9225),  
   heavy-vehicles                             (9226),  
   trucks                                     (9227),  
   buses                                      (9228),  
   articulated-buses                          (9229),  
   school-buses                               (9230),  
   vehicles-with-semi-trailers                (9231),  
   vehicles-with-double-trailers              (9232),  -- Alternative Rendering:  western
                                                       -- doubles
   high-profile-vehicles                      (9233),  
   wide-vehicles                              (9234),  
   long-vehicles                              (9235),  
   hazardous-loads                            (9236),  
   exceptional-loads                          (9237),  
   abnormal-loads                             (9238),  
   convoys                                    (9239),  
   maintenance-vehicles                       (9240),  
   delivery-vehicles                          (9241),  
   vehicles-with-even-numbered-license-plates (9242),  
   vehicles-with-odd-numbered-license-plates  (9243),  
   vehicles-with-parking-permits              (9244),  
   vehicles-with-catalytic-converters         (9245),  
   vehicles-without-catalytic-converters      (9246),  
   gas-powered-vehicles                       (9247),  
   diesel-powered-vehicles                    (9248),  
   lPG-vehicles                               (9249),  
   military-convoys                           (9250),  
   military-vehicles                          (9251),  
   ... -- # LOCAL_CONTENT_ITIS 
   }
   -- Classification of vehicles and types of transport



-- Unable to find the file: ITISstubs.txt
-- Which would be be inserted at this point if present.


 
END
-- End of the ITIS module.
 
 
-- No entries were found with unknown module assignments.
 


-- End of file output at 11/11/2009 1:15:00 PM
