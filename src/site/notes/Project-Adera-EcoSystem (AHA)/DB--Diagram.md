---
{"dg-publish":true,"dg-path":"Project-Adera-EcoSystem (AHA)/DB--Diagram.md","permalink":"/Project-Adera-EcoSystem (AHA)/DB--Diagram/","dgPassFrontmatter":true,"noteIcon":""}
---



xml file
<mxGraphModel dx="1426" dy="754" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="850" pageHeight="1100" math="0" shadow="0">
  <root>
    <mxCell id="0"/>
    <mxCell id="1" parent="0"/>
    <mxCell id="users" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>users</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>email</td><td>text</td></tr><tr><td>phone</td><td>text</td></tr><tr><td>role</td><td>user_role</td></tr><tr><td>first_name, last_name</td><td>text</td></tr><tr><td>location</td><td>point</td></tr><tr><td>...</td><td></td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="160" y="40" width="220" height="160" as="geometry"/>
    </mxCell>
    <mxCell id="shops" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>shops</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>owner_id (FK)</td><td>uuid → users</td></tr><tr><td>name, category</td><td>text</td></tr><tr><td>location</td><td>point</td></tr><tr><td>is_active, is_verified</td><td>boolean</td></tr><tr><td>delivery_radius</td><td>integer</td></tr><tr><td>...</td><td></td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="480" y="40" width="220" height="160" as="geometry"/>
    </mxCell>
    <mxCell id="products" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>products</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>shop_id (FK)</td><td>uuid → shops</td></tr><tr><td>name, category, sku</td><td>text</td></tr><tr><td>price, stock_quantity</td><td>numeric, integer</td></tr><tr><td>is_available</td><td>boolean</td></tr><tr><td>...</td><td></td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="480" y="240" width="220" height="140" as="geometry"/>
    </mxCell>
    <mxCell id="orders" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>orders</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>customer_id (FK)</td><td>uuid → users</td></tr><tr><td>shop_id (FK)</td><td>uuid → shops</td></tr><tr><td>order_number</td><td>text (UNIQUE)</td></tr><tr><td>total_amount</td><td>numeric</td></tr><tr><td>status, payment_status</td><td>text, payment_status</td></tr><tr><td>parcel_id (FK)</td><td>uuid → parcels</td></tr><tr><td>...</td><td></td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="160" y="280" width="220" height="180" as="geometry"/>
    </mxCell>
    <mxCell id="order_items" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>order_items</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>order_id (FK)</td><td>uuid → orders</td></tr><tr><td>product_id (FK)</td><td>uuid → products</td></tr><tr><td>product_name, product_price</td><td>text, numeric</td></tr><tr><td>quantity, item_total</td><td>integer, numeric</td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="160" y="520" width="220" height="140" as="geometry"/>
    </mxCell>
    <mxCell id="parcels" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>parcels</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>sender_id (FK)</td><td>uuid → users</td></tr><tr><td>driver_id, pickup/dropoff_partner_id</td><td>uuid → users</td></tr><tr><td>pickup/dropoff_shop_id</td><td>uuid → shops</td></tr><tr><td>tracking_id</td><td>text (UNIQUE)</td></tr><tr><td>status, payment_status</td><td>int, payment_status</td></tr><tr><td>delivery_location, pickup_location</td><td>point</td></tr><tr><td>...</td><td></td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="480" y="480" width="240" height="200" as="geometry"/>
    </mxCell>
    <mxCell id="parcel_events" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>parcel_events</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>parcel_id (FK)</td><td>uuid → parcels</td></tr><tr><td>actor_id (FK)</td><td>uuid → users</td></tr><tr><td>actor_role</td><td>user_role</td></tr><tr><td>status, event_time</td><td>int, timestamptz</td></tr><tr><td>location, photos, signature_url</td><td>point, text[], text</td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="780" y="480" width="220" height="160" as="geometry"/>
    </mxCell>
    <mxCell id="payments" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>payments</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>user_id (FK)</td><td>uuid → users</td></tr><tr><td>order_id (FK)</td><td>uuid → orders</td></tr><tr><td>parcel_id (FK)</td><td>uuid → parcels</td></tr><tr><td>amount, currency</td><td>numeric, text</td></tr><tr><td>payment_method, payment_status</td><td>payment_method, payment_status</td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="780" y="280" width="220" height="160" as="geometry"/>
    </mxCell>
    <mxCell id="reviews" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>reviews</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>user_id (FK)</td><td>uuid → users</td></tr><tr><td>product_id (FK)</td><td>uuid → products</td></tr><tr><td>rating (1–5)</td><td>integer</td></tr><tr><td>comment</td><td>text</td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="480" y="720" width="180" height="120" as="geometry"/>
    </mxCell>
    <mxCell id="notifications" value="<div style=&quot;box-sizing: border-box ; width: 100% ; background: #e4e4e4 ; padding: 2px&quot;>notifications</div><table style=&quot;width: 100% ; font-size: 1em&quot; cellpadding=&quot;2&quot; cellspacing=&quot;0&quot;><tbody><tr><td>id (PK)</td><td>uuid</td></tr><tr><td>user_id (FK)</td><td>uuid → users</td></tr><tr><td>title, body, type</td><td>text</td></tr><tr><td>reference_id, reference_type</td><td>uuid, text</td></tr><tr><td>is_read, is_push_sent, etc.</td><td>boolean</td></tr></tbody></table>" style="verticalAlign=top;align=left;overflow=fill;html=1;labelBackgroundColor=#ffffff;" parent="1" vertex="1">
      <mxGeometry x="160" y="720" width="220" height="140" as="geometry"/>
    </mxCell>
    <!-- Relationships -->
    <mxCell id="edge1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="users" target="shops" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;jettySize=auto;orthogonalLoop=1;" parent="1" source="shops" target="products" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge3" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0;exitY=0.5;entryX=1;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="orders" target="users" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge4" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.25;entryX=0;entryY=0.75;jettySize=auto;orthogonalLoop=1;" parent="1" source="orders" target="shops" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge5" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;jettySize=auto;orthogonalLoop=1;" parent="1" source="orders" target="order_items" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge6" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.25;jettySize=auto;orthogonalLoop=1;" parent="1" source="order_items" target="products" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge7" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.75;entryX=0;entryY=0.25;jettySize=auto;orthogonalLoop=1;" parent="1" source="orders" target="parcels" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge8" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0;exitY=0.25;entryX=1;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="parcels" target="users" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge9" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0;exitY=0.75;entryX=1;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="parcels" target="shops" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge10" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="parcels" target="parcel_events" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge11" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=0;entryX=0.5;entryY=1;jettySize=auto;orthogonalLoop=1;" parent="1" source="parcel_events" target="users" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge12" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.25;entryX=0;entryY=0.75;jettySize=auto;orthogonalLoop=1;" parent="1" source="payments" target="users" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge13" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0;exitY=0.25;entryX=1;entryY=0.75;jettySize=auto;orthogonalLoop=1;" parent="1" source="payments" target="orders" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge14" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0;exitY=0.75;entryX=1;entryY=0.25;jettySize=auto;orthogonalLoop=1;" parent="1" source="payments" target="parcels" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge15" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;jettySize=auto;orthogonalLoop=1;" parent="1" source="products" target="reviews" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge16" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.75;entryX=0;entryY=0.5;jettySize=auto;orthogonalLoop=1;" parent="1" source="reviews" target="users" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
    <mxCell id="edge17" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;jettySize=auto;orthogonalLoop=1;" parent="1" source="users" target="notifications" edge="1">
      <mxGeometry relative="1" as="geometry"/>
    </mxCell>
  </root>
</mxGraphModel>


High-level Representations

![[Pasted image 20251126233205.png\|Pasted image 20251126233205.png]]