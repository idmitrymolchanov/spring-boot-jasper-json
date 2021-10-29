# spring-boot-jasper-json
Fill Jasper .jrxml with JsonDataSource

## 1 Simple Report
1.1 form json and post it

json:

    {
      "value1":"string1",
      "key1":"key"
    }

1.2 after receiving the request body in the controller send the body in a convenient format to the service layer (Map<,> for example)

1.3 format your structure (Map<,>) to json string. Use ObjectMapper, for example

1.4 service:  
// jsonData = json string from 1.3

    ByteArrayInputStream jsonDataStream = new ByteArrayInputStream(jsonData.getBytes());
    JsonDataSource dataSource;
    try {
      dataSource = new JsonDataSource(jsonDataStream);
    } catch (JRException e) {}
    
1.5 Fill jasper template ([sodocumentation.net](https://sodocumentation.net/jasper-reports/topic/4190/export-to-pdf))

    JasperFillManager.fillReport(jasperReport, parameters, dataSource);
    
1.6 create simple .jrxml
    
    <?xml version="1.0" encoding="UTF-8"?>
    <jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd" name="example" pageWidth="595" pageHeight="842" columnWidth="535" leftMargin="20" rightMargin="20" topMargin="20" bottomMargin="20" uuid="ea9d9eab-78ea-4cb9-adce-7588ae6a483e">

    <field name="value1" class="java.lang.Integer">
    </field>
    <field name="key1" class="java.lang.String">
    </field>
    <background>
        <band/>
    </background>
    <columnHeader>
        <band height="21">
            <line>
                <reportElement x="-20" y="20" width="595" height="1" forecolor="#666666" uuid="ca533965-9162-4dc2-8a9e-2c99b0db8e3e"/>
            </line>
            <staticText>
                <reportElement mode="Opaque" x="0" y="0" width="111" height="20" forecolor="#006699" backcolor="#E6E6E6" uuid="55735968-342c-4d29-8dc1-db2263a44d11">
                    <property name="com.jaspersoft.studio.spreadsheet.connectionID" value="40a9814d-ecb4-424f-aea8-cad246f112b5"/>
                </reportElement>
                <textElement textAlignment="Center">
                    <font size="14" isBold="true"/>
                </textElement>
                <text><![CDATA[Value1]]></text>
            </staticText>
            <staticText>
                <reportElement mode="Opaque" x="111" y="0" width="111" height="20" forecolor="#006699" backcolor="#E6E6E6" uuid="e9b64414-ad92-4257-a8f2-63a23c460255">
                    <property name="com.jaspersoft.studio.spreadsheet.connectionID" value="372fc4a5-58e8-4f68-9b0b-b8c92b4521d4"/>
                </reportElement>
                <textElement textAlignment="Center">
                    <font size="14" isBold="true"/>
                </textElement>
                <text><![CDATA[Key1]]></text>
            </staticText>
        </band>
    </columnHeader>
    <detail>
        <band height="20">
            <line>
                <reportElement positionType="FixRelativeToBottom" x="0" y="19" width="555" height="1" uuid="305d3455-6f0a-49e7-ad2a-055974675fd4"/>
            </line>
            <textField isStretchWithOverflow="true">
                <reportElement x="0" y="0" width="111" height="20" uuid="1324277f-1cf1-4cc2-a5f3-88ededc0ff6e">
                    <property name="com.jaspersoft.studio.spreadsheet.connectionID" value="40a9814d-ecb4-424f-aea8-cad246f112b5"/>
                </reportElement>
                <textElement textAlignment="Center">
                    <font size="14"/>
                </textElement>
                <textFieldExpression><![CDATA[$F{value1}]]></textFieldExpression>
            </textField>
            <textField isStretchWithOverflow="true">
                <reportElement x="111" y="0" width="111" height="20" uuid="b9c4d404-6ffe-47b5-aeaf-ee9ffb804c6a">
                    <property name="com.jaspersoft.studio.spreadsheet.connectionID" value="372fc4a5-58e8-4f68-9b0b-b8c92b4521d4"/>
                </reportElement>
                <textElement textAlignment="Center">
                    <font size="14"/>
                </textElement>
                <textFieldExpression><![CDATA[$F{key1}]]></textFieldExpression>
            </textField>
        </band>
    </detail>
    <summary>
        <band/>
    </summary>
    </jasperReport>

## 2 Jasper report with table and subDataset (nested json)

2.1 json with nested datasets

  {    
      "key" : "first_key",
      "one":
          {
            "value1" : "string1",
            "key1" : "second_key"
          }
  }

2.2 service (p. 1.4)

2.3 .jrxml

    <?xml version="1.0" encoding="UTF-8"?>
    <jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd" name="TableReport" pageWidth="595" pageHeight="842" whenNoDataType="AllSectionsNoDetail" columnWidth="555" leftMargin="20" rightMargin="20" topMargin="20" bottomMargin="20" uuid="80e30745-0db9-476f-8bbb-cbc60d1ca90f">
    <property name="com.jaspersoft.studio.data.defaultdataadapter"/>
    <style name="Table_CH" mode="Opaque" backcolor="#BFE1FF">
      <box>
        <pen lineWidth="0.5" lineColor="#000000"/>
        <topPen lineWidth="0.5" lineColor="#000000"/>
        <leftPen lineWidth="0.5" lineColor="#000000"/>
        <bottomPen lineWidth="0.5" lineColor="#000000"/>
        <rightPen lineWidth="0.5" lineColor="#000000"/>
      </box>
    </style>
    <style name="Table_TD" mode="Opaque" backcolor="#FFFFFF">
      <box>
        <pen lineWidth="0.5" lineColor="#000000"/>
        <topPen lineWidth="0.5" lineColor="#000000"/>
        <leftPen lineWidth="0.5" lineColor="#000000"/>
        <bottomPen lineWidth="0.5" lineColor="#000000"/>
        <rightPen lineWidth="0.5" lineColor="#000000"/>
      </box>
    </style>
    <subDataset name="Dataset1" uuid="7918eaef-d2f7-4cdd-9b84-e2660b2bfed8">
      <parameter name="mainDatasource" class="net.sf.jasperreports.engine.data.JsonDataSource"/>
      <queryString language="JSON">
        <![CDATA[]]>
      </queryString>
      <field name="key" class="java.lang.String">
        <fieldDescription><![CDATA[key]]></fieldDescription>
      </field>
    </subDataset>
    <parameter name="JSON_INPUT_STREAM" class="java.io.InputStream" isForPrompting="false"/>
    <queryString language="JSON">
      <![CDATA[]]>
    </queryString>
    <field name="key" class="java.lang.String">
      <fieldDescription><![CDATA[key]]></fieldDescription>
    </field>
    <title>
      <band height="79" splitType="Stretch">
        <textField>
           <reportElement x="0" y="0" width="200" height="40"/>
          <textElement textAlignment="Center" verticalAlignment="Middle"/>
          <textFieldExpression><![CDATA[$F{key}]]></textFieldExpression>
        </textField>
      </band>
    </title>
    <summary>
      <band height="260" splitType="Stretch">
        <componentElement>
          <reportElement x="0" y="0" width="200" height="60" uuid="40093b50-bd8d-4d56-8d1d-9f2e9cf86106">
            <property name="com.jaspersoft.studio.layout" value="com.jaspersoft.studio.editor.layout.VerticalRowLayout"/>
            <property name="com.jaspersoft.studio.table.style.column_header" value="Table_CH"/>
            <property name="com.jaspersoft.studio.table.style.detail" value="Table_TD"/>
          </reportElement>
          <jr:table xmlns:jr="http://jasperreports.sourceforge.net/jasperreports/components" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports/components http://jasperreports.sourceforge.net/xsd/components.xsd">
            <datasetRun subDataset="Dataset1" uuid="0247b2fb-527b-4dc2-af7f-9bcd75e8715a">
              <datasetParameter name="mainDatasource">
                <datasetParameterExpression><![CDATA[((net.sf.jasperreports.engine.data.JsonDataSource)$P{REPORT_DATA_SOURCE})]]></datasetParameterExpression>
              </datasetParameter>
              <dataSourceExpression><![CDATA[((net.sf.jasperreports.engine.data.JsonDataSource)$P{REPORT_DATA_SOURCE}).subDataSource("one")]]>.</dataSourceExpression>
            </datasetRun>
            <jr:column width="200" uuid="25910003-959c-4e31-a91e-555e4aaf0bec">
              <jr:detailCell style="Table_TD" height="30">
                <textField>
                  <reportElement x="0" y="0" width="200" height="30" uuid="24b2ba9b-57d0-4840-8333-70247f595e21"/>
                  <textElement textAlignment="Center" verticalAlignment="Middle"/>
                  <textFieldExpression><![CDATA[$F{value1}]]></textFieldExpression>
                </textField>
                <textField>
                  <reportElement x="0" y="50" width="200" height="30" uuid="24b2ba9b-57d0-4840-8333-70247f595e21"/>
                  <textElement textAlignment="Center" verticalAlignment="Middle"/>
                  <textFieldExpression><![CDATA[$F{key1}]]></textFieldExpression>
                </textField>
              </jr:detailCell>
            </jr:column>
          </jr:table>
        </componentElement>
      </band>
    </summary>
   </jasperReport>


## 3 Jasper report with table and subDataset (line json)

2 Simple json

    {
      "value1" : "string1",
      "value2" : "string2",
      "value3" : "string3",
      "key1" : "key",
    }

    <datasetRun subDataset="Dataset1" uuid="0247b2fb-527b-4dc2-af7f-9bcd75e8715a">
        <datasetParameter name="mainDatasource">
              <datasetParameterExpression><![CDATA[((net.sf.jasperreports.engine.data.JsonDataSource)$P{REPORT_DATA_SOURCE})]]></datasetParameterExpression>
        </datasetParameter>
        <dataSourceExpression><![CDATA[((net.sf.jasperreports.engine.data.JsonDataSource)$P{REPORT_DATA_SOURCE}).subDataSource("")]]></dataSourceExpression>
    </datasetRun>
