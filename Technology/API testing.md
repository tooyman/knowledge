```json
swagger: '2.0'
info:
  description: SCB Enterprise RESTful API for CASA project
  version: '1.0'
  title: SCB Enterprise RESTful API (CASA project)
  termsOfService: https://www.scb.co.th
  contact:
    name: SCB Enterprise API Support
    url: https://www.scb.co.th
    email: ea_api@scb.co.th
  license:
    name: Apache License Version 2.0
    url: https://github.com/IBM-Bluemix/news-aggregator/blob/master/LICENSE
host: intapigw-dev-clientcrt.se.scb.co.th:8448
basePath: /scb/rest/ent-api
tags:
  - name: External-Agency.DETICA
schemes:
  - https
consumes:
  - application/json
produces:
  - application/json
paths:
  /v1/external-agency/detica/blacklist/validation:
    post:
      tags:
        - External-Agency.DETICA
      summary: DETICA Blacklist validation
      description: This API is used for validating the specified customer profiles against the DETICA's Blacklist.
      operationId: validateBlacklist
      consumes:
        - application/json
      produces:
        - application/json
      parameters:
        - name: apiKey
          in: header
          description: The apiKey used to authenticate access
          required: true
          type: string
          default: 128ed17x-f016-4bc6-b11b-c63bc08bbc1x
        - name: apiSecret
          in: header
          description: The apiSecret used to authenticate access
          required: true
          type: string
          default: b601198954cd85af1888f072c02f7e91
        - name: requestUID
          in: header
          description: requestUID
          required: true
          type: string
          default: 7fc8c476-45f1-4481-9598-9a3ae7c9018b
        - name: resourceOwnerID
          in: header
          description: resourceOwnerID
          required: true
          type: string
          default: ENET
        - in: body
          name: validationReq
          description: validationReq
          required: true
          schema:
            type: array
            items:
              $ref: '#/definitions/DeticaValidationRequest'
      responses:
        '200':
          description: Success
          schema:
            type: array
            items:
              $ref: '#/definitions/DeticaValidationResponse'
        '201':
          description: Created
        '400':
          description: Bad Request
          schema:
            $ref: '#/definitions/Errors'
        '401':
          description: Unauthorized
          schema:
            $ref: '#/definitions/Errors'
        '403':
          description: Forbidden
          schema:
            $ref: '#/definitions/Errors'
        '404':
          description: Not Found
        '500':
          description: Internal server error occurred
          schema:
            $ref: '#/definitions/Errors'
        '503':
          description: Service Unavailable
          schema:
            $ref: '#/definitions/Errors'
definitions:
  DeticaValidationHitResult:
    type: object
    required:
      - alertID
      - hitNumber
      - message
      - score
    properties:
      hitNumber:
        type: integer
        format: int32
        example: 6051
        description: Hit number (1-based)
      alertID:
        type: string
        example: A140500003015
        description: The alert number in DETICA. One customer has one alert number In each hitresults. Note that the alert number is the same for a single customer.
      message:
        type: string
        example: Hit on AMLO High Risk List
        description: 'Details of the matching validation result. There are 2 types: The HIT that does not allow entity to conduct transaction with the bank (Hit on AMLO Freeze04 List, Hit on AMLO Freeze05 List, Hit on WORLD CHECK NON-UN SANCTIONS GROUP LIST, Hit on WORLD CHECK UN SANCTIONS GROUP LIST) and the HIT that still allow entity to conduct transaction with the bank (Hit on AMLO High Risk List, Hit on WORLDCHECK)'
      idNumber:
        type: string
        example: A140600035021
        description: Identification number (Citizen Identification Number/Business Registration Number/Passport Number)
      thaiName:
        type: string
        example: มนตรี กลั่นเกษม
        description: Matched entitiy name in Thai
      engName:
        type: string
        example: Montri Karnkasem
        description: Matched entitiy name in Entity
      score:
        type: string
        example: 100%
        description: The matching score percentage (100% maximum)
    description: DETICA's blacklist validation matching result information
  Errors:
    type: object
    required:
      - errors
    properties:
      errors:
        type: array
        description: List of ErrorInfo
        items:
          $ref: '#/definitions/ErrorInfo'
    description: List of Error Information
  ErrorInfo:
    type: object
    required:
      - code
      - description
      - message
      - severitylevel
    properties:
      code:
        type: string
        example: '1234'
        description: Error Code
      message:
        type: string
        example: Missing juristicId query parameter
        description: Error Message
      severitylevel:
        type: string
        example: Error
        description: SeverityLevel Code ("ERROR", "WARNING", "INFO")
      description:
        type: string
        example: The juristicId query parameter is required to prevent accidentally invoking API to retrieve all data records.
        description: Description of the problem with hints about how to fix it
      moreInfo:
        type: string
        example: 'ORIGINAL ERROR: E1234 MISSING JURISTIC-ID'
        description: More information on the problem and solution. Or, the error message from backend.
      originalErrorCode:
        type: string
        example: RM7885
        description: Original error code from backend.
      originalErrorDesc:
        type: string
        example: Credit Card Not found
        description: Original error message from backend.
    description: Detailed of Error
  DeticaValidationRequest:
    type: object
    required:
      - customerTypeCode
      - organization
    properties:
      custID:
        type: string
        example: '3166534253455'
        description: Citizen Identification Number / Business Registration Number / Passport (ID COUNTRY(2) + Passport NO)
      thaiName:
        type: string
        example: สมชาย แซ่ตั้ง
        description: First Name & space & Last Name in Thai
      engName:
        type: string
        example: Somchai Saetang
        description: First Name & space & Last Name in English
      countryCode:
        type: string
        example: TH
        description: Country according to official registered address of customer (ISO 3166)
      countryName:
        type: string
        example: THAILAND
        description: Country name
      birthDate:
        type: string
        example: '1975-02-10'
        description: Birth date (YYYY-MM-DD)
      placeOfBirth:
        type: string
        example: ภูเก็ต
        description: Place of birth
      idNumber:
        type: string
        example: '11263265222'
        description: Identification number (Citizen Identification Number/Business Registration Number/Passport Number)
      address:
        type: string
        example: 34/24 ถ.พหลโยธิน, พญาไท, กรุงเทพฯ 10400
        description: Official registered address of customer (person or business)
      customerTypeCode:
        type: string
        example: P
        description: 'Customer type code (P: Individual , C: Corporate)'
      organization:
        type: string
        example: RTL
        description: 'Alerting to organization (RTL: Retail, WBG: Wholesales Banking Group, BBG: Business Banking Group, SAG: Special Asset Group, OTH: Other, TS: Financial Institution, VIP: VIP Customer, STF: Staff, SHA: Shared Org unit view to all)'
    description: Inputs for DETICA's blacklist validation
  DeticaValidationResponse:
    type: object
    required:
      - recordNumber
      - resultCode
    properties:
      recordNumber:
        type: integer
        format: int32
        example: 0
        description: Record number (0-based)
      resultCode:
        type: string
        example: HIT
        description: 'Validation result code (HIT: Matched, NOHIT: Not matched)'
      hitResults:
        type: array
        description: Details of validation result (only available when result code = HIT)
        items:
          $ref: '#/definitions/DeticaValidationHitResult'
    description: DETICA's blacklist validation result
responses:
  '200':
    description: Success
    schema:
      type: array
      items:
        $ref: '#/definitions/DeticaValidationResponse'
  '201':
    description: Created
  '400':
    description: Bad Request
    schema:
      $ref: '#/definitions/Errors'
  '401':
    description: Unauthorized
    schema:
      $ref: '#/definitions/Errors'
  '403':
    description: Forbidden
    schema:
      $ref: '#/definitions/Errors'
  '404':
    description: Not Found
  '500':
    description: Internal server error occurred
    schema:
      $ref: '#/definitions/Errors'
  '503':
    description: Service Unavailable
    schema:
      $ref: '#/definitions/Errors'


```
