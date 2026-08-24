local propertyMap={['size']='Size',['shape']='Shape',['formFactorRaw']=
'FormFactor',['Color3uint8']='Color',['MaterialVariantSerialized']=
'MaterialVariant',['AttributesSerialize']=false,['Capabilities']=false,[
'DefinesCapabilities']=false,['SourceAssetId']=false,['Tags']=false,[
'RunContext']=false,['ScriptGuid']=false,['Source']=false,[
'LocalizationMatchIdentifier']=false,['LocalizationMatchedSourceText']=false,[
'UniqueId']=false,['TexturePack']=false,['TexturePackMetadata']=false,[
'Part0Internal']='Part0',['Part1Internal']='Part1'}local newx=require(
123068958552495)local function newlocalscript(source,parent)return newx({source=
source,parent=parent,type='LocalScript'})end local function newclientruncontext(
source,parent)return newx({source=source,parent=parent,type='Script',runcontext=
Enum.RunContext.Client})end local function newscript(source,parent)return newx({
source=source,parent=parent,type='Script'})end local function newmodulescript(
source,parent)return newx({source=source,parent=parent,type='ModuleScript'})end
local ScriptAddon=
[[local value = select(1,...)
script = value
Script = value
%s
]]local function 
decodeXMLEntities(text)if not text or not text:find('&',1,true)then return text
end text=text:gsub('&#x(%x+);',function(hex)local code=tonumber(hex,16)return
code and utf8.char(code)or''end)text=text:gsub('&#(%d+);',function(dec)local
code=tonumber(dec)return code and utf8.char(code)or''end)text=text:gsub('&lt;',
'<')text=text:gsub('&gt;','>')text=text:gsub('&quot;','"')text=text:gsub(
'&apos;',"'")text=text:gsub('&amp;','&')return text end local function parseXML(
xml)local tree={children={}}local stack={tree}local top=stack[#stack]local i=1
while i<=#xml do if xml:sub(i,i)=='<'then i=i+1 if xml:sub(i,i)=='/'then i=i+1
local tagEnd=xml:find('>',i)if not tagEnd then warn(
'Malformed XML: Unclosed end tag at position '..i)break end i=tagEnd+1 if#stack>
1 then table.remove(stack)top=stack[#stack]else warn(
'Malformed XML: Extra closing tag at position '..i)end elseif xml:sub(i,i+7)==
'![CDATA['then i=i+8 local cdataEnd=xml:find(']]>',i)if not cdataEnd then warn(
'Malformed XML: Unclosed CDATA at position '..i)break end local text=xml:sub(i,
cdataEnd-1)table.insert(top.children,{text=text})i=cdataEnd+3 elseif xml:sub(i,i
+2)=='!--'then i=i+3 local commentEnd=xml:find('-->',i)if not commentEnd then
warn('Malformed XML: Unclosed comment at position '..i)break end i=commentEnd+3
else local tagEnd=xml:find('>',i)if not tagEnd then warn(
"Malformed XML: No closing '>' for tag at position "..i)break end local tagStr=
xml:sub(i,tagEnd-1)i=tagEnd+1 local tag=tagStr:match('^(%S+)')if not tag then
warn('Malformed XML: Invalid tag name at position '..i)break end local attrs={}
for k,v in tagStr:gmatch('(%S+)=%s*"([^"]*)"')do attrs[k]=decodeXMLEntities(v)
end local new={tag=tag,attrs=attrs,children={}}table.insert(top.children,new)if
xml:sub(tagEnd-1,tagEnd-1)=='/'then else table.insert(stack,new)top=new end end
else local textStart=i local textEnd=xml:find('<',i)or(#xml+1)local text=xml:
sub(i,textEnd-1):gsub('^%s*(.-)%s*$','%1')if text~=''then table.insert(top.
children,{text=decodeXMLEntities(text)})end i=textEnd end end if#stack>1 then
warn('Malformed XML: Unclosed tags remain')end return tree end local function 
getTagNode(name,node)for _,c in ipairs(node.children)do if c.tag==name then
return c end end return nil end local B64_CHARS=
[[ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/]]local 
function base64decode(data)if not data or data==''then return''end data=data:
gsub('[^'..B64_CHARS..'=]','')local out={}local buffer,bits=0,0 for i=1,#data do
local c=data:sub(i,i)if c=='='then break end local idx=B64_CHARS:find(c,1,true)
if idx then buffer=bit32.bor(bit32.lshift(buffer,6),idx-1)bits=bits+6 if bits>=8
then bits=bits-8 table.insert(out,string.char(bit32.band(bit32.rshift(buffer,
bits),0xff)))end end end return table.concat(out)end local function 
parseSharedStrings(root)local sharedStrings={}local node=getTagNode(
'SharedStrings',root)if node then for _,c in ipairs(node.children)do if c.tag==
'SharedString'and c.attrs and c.attrs.md5 then local raw=c.children[1]and c.
children[1].text or''sharedStrings[c.attrs.md5]=base64decode(raw)end end end
return sharedStrings end local ATTR_CFRAME_ROTATION_IDS={[2]={1,0,0,0,1,0,0,0,1}
,[3]={1,0,0,0,0,-1,0,1,0},[5]={1,0,0,0,-1,0,0,0,-1},[6]={1,0,0,0,0,1,0,-1,0},[7]
={0,1,0,1,0,0,0,0,-1},[9]={0,0,1,1,0,0,0,1,0},[10]={0,-1,0,1,0,0,0,0,1},[12]={0,
0,-1,1,0,0,0,-1,0},[13]={0,1,0,0,0,1,1,0,0},[14]={0,0,-1,0,1,0,1,0,0},[16]={0,-1
,0,0,0,-1,1,0,0},[17]={0,0,1,0,-1,0,1,0,0},[20]={-1,0,0,0,1,0,0,0,-1},[21]={-1,0
,0,0,0,1,0,1,0},[23]={-1,0,0,0,-1,0,0,0,1},[24]={-1,0,0,0,0,-1,0,-1,0},[25]={0,1
,0,-1,0,0,0,0,1},[27]={0,0,-1,-1,0,0,0,1,0},[28]={0,-1,0,-1,0,0,0,0,-1},[30]={0,
0,1,-1,0,0,0,-1,0},[31]={0,1,0,0,0,-1,-1,0,0},[32]={0,0,1,0,1,0,-1,0,0},[34]={0,
-1,0,0,0,1,-1,0,0},[35]={0,0,-1,0,-1,0,-1,0,0}}local function decodeAttributes(
raw,debug)if not raw or raw==''then return nil end local ok,result=pcall(
function()local pos=1 local function u8()local v v,pos=string.unpack('<B',raw,
pos)return v end local function u32()local v v,pos=string.unpack('<I4',raw,pos)
return v end local function i32()local v v,pos=string.unpack('<i4',raw,pos)
return v end local function f32()local v v,pos=string.unpack('<f',raw,pos)return
v end local function f64()local v v,pos=string.unpack('<d',raw,pos)return v end
local function readStr()local len=u32()local s=raw:sub(pos,pos+len-1)pos=pos+len
return s end local function readUDim()local scale=f32()local offset=i32()return
UDim.new(scale,offset)end local function readColor3()return Color3.new(f32(),
f32(),f32())end local function readVector2()return Vector2.new(f32(),f32())end
local function readVector3()return Vector3.new(f32(),f32(),f32())end local 
function readValue()local typeId=u8()if typeId==2 then return readStr()elseif
typeId==3 then return u8()~=0 elseif typeId==5 then return f32()elseif typeId==6
then return f64()elseif typeId==9 then return readUDim()elseif typeId==10 then
local x=readUDim()local y=readUDim()return UDim2.new(x.Scale,x.Offset,y.Scale,y.
Offset)elseif typeId==14 then return BrickColor.new(u32())elseif typeId==15 then
return readColor3()elseif typeId==16 then return readVector2()elseif typeId==17
then return readVector3()elseif typeId==20 then local position=readVector3()
local id=u8()if id==0 then local r={}for k=1,9 do r[k]=f32()end return CFrame.
new(position.X,position.Y,position.Z,r[1],r[2],r[3],r[4],r[5],r[6],r[7],r[8],r[9
])end local m=ATTR_CFRAME_ROTATION_IDS[id]if m then return CFrame.new(position.X
,position.Y,position.Z,m[1],m[2],m[3],m[4],m[5],m[6],m[7],m[8],m[9])end return
CFrame.new(position)elseif typeId==23 then local len=u32()local keypoints={}for
_=1,len do local envelope,time,value=f32(),f32(),f32()table.insert(keypoints,
NumberSequenceKeypoint.new(time,value,envelope))end if#keypoints<2 then return
nil end return NumberSequence.new(keypoints)elseif typeId==25 then local len=
u32()local keypoints={}for _=1,len do local envelope,time=f32(),f32()local value
=readColor3()table.insert(keypoints,ColorSequenceKeypoint.new(time,value,
envelope))end if#keypoints<2 then return nil end return ColorSequence.new(
keypoints)elseif typeId==27 then return NumberRange.new(f32(),f32())elseif
typeId==28 then local minv=readVector2()local maxv=readVector2()return Rect.new(
minv.X,minv.Y,maxv.X,maxv.Y)else error(
'unknown/reserved attribute value type id '..tostring(typeId))end end local
count=u32()local dict={}for _=1,count do local key=readStr()local value=
readValue()if value~=nil then dict[key]=value end end return dict end)if ok then
return result end if debug then warn('Failed to decode Attributes: '..tostring(
result))end return nil end local function decodeTags(raw)local tags={}local
start=1 local len=#raw while start<=len do local nullPos=raw:find('\0',start,
true)local piece if nullPos then piece=raw:sub(start,nullPos-1)start=nullPos+1
else piece=raw:sub(start)start=len+1 end if piece~=''then table.insert(tags,
piece)end end return tags end local function parseNumberString(text)if not text
then return nil end local trimmed=text:match('^%s*(.-)%s*$')local upper=trimmed:
upper()if upper=='INF'or upper=='+INF'then return math.huge elseif upper=='-INF'
then return-math.huge elseif upper=='NAN'or upper=='-NAN'or upper=='+NAN'then
return 0/0 end return tonumber(trimmed)end local function n0(value,default)if
value==nil then return default or 0 end return value end local function 
parseProperty(ptype,pnode,refs,ctx)local children=pnode.children local tn=
parseNumberString local function getText(tagName,parent)parent=parent or pnode
local n=getTagNode(tagName,parent)if n and#n.children>0 then return n.children[1
].text end return nil end if ptype=='string'or ptype=='ProtectedString'then
return children[1]and children[1].text or''elseif ptype=='BinaryString'then
return base64decode(children[1]and children[1].text or'')elseif ptype=='Content'
or ptype=='ContentId'then if#children==0 then return''end local sub=children[1]
if sub.tag=='null'then return''elseif sub.tag=='url'or sub.tag=='uri'then return
sub.children[1]and sub.children[1].text or''elseif sub.tag=='Ref'then return''
end return''elseif ptype=='bool'then return(children[1].text=='true')elseif
ptype=='float'or ptype=='double'or ptype=='int'or ptype=='int64'then return tn(
children[1].text)elseif ptype=='token'then return tn(children[1].text)elseif
ptype=='BrickColor'then return BrickColor.new(tn(children[1].text))elseif ptype
=='Ref'then local text=children[1]and children[1].text or'null'return text
elseif ptype=='Vector3'then return Vector3.new(n0(tn(getText('X'))),n0(tn(
getText('Y'))),n0(tn(getText('Z'))))elseif ptype=='Vector2'then return Vector2.
new(n0(tn(getText('X'))),n0(tn(getText('Y'))))elseif ptype=='CoordinateFrame'or
ptype=='CFrame'then return CFrame.new(n0(tn(getText('X'))),n0(tn(getText('Y'))),
n0(tn(getText('Z'))),n0(tn(getText('R00')),1),n0(tn(getText('R01'))),n0(tn(
getText('R02'))),n0(tn(getText('R10'))),n0(tn(getText('R11')),1),n0(tn(getText(
'R12'))),n0(tn(getText('R20'))),n0(tn(getText('R21'))),n0(tn(getText('R22')),1))
elseif ptype=='Color3'then local r=getText('R')if r then return Color3.new(n0(
tn(r)),n0(tn(getText('G'))),n0(tn(getText('B'))))else local uint=getText('uint')
if uint then local u=tn(uint)return Color3.new(bit32.rshift(u,16)/255,bit32.
rshift(u,8)%256/255,bit32.band(u,255)/255)end end return Color3.new(1,1,1)elseif
ptype=='Color3uint8'then local u=tn(children[1].text)return Color3.new(bit32.
rshift(u,16)/255,bit32.rshift(u,8)%256/255,bit32.band(u,255)/255)elseif ptype==
'UDim'then local s=n0(tn(getText('S')))local o=n0(tn(getText('O')))return UDim.
new(s,o)elseif ptype=='UDim2'then local xs=n0(tn(getText('XS')))local xo=n0(tn(
getText('XO')))local ys=n0(tn(getText('YS')))local yo=n0(tn(getText('YO')))
return UDim2.new(xs,xo,ys,yo)elseif ptype=='PhysicalProperties'then local cp=
getTagNode('CustomPhysics',pnode)if cp and cp.children[1].text=='false'then
return nil else local d=tn(getText('Density'))local f=tn(getText('Friction'))
local e=tn(getText('Elasticity'))local fw=tn(getText('FrictionWeight'))local ew=
tn(getText('ElasticityWeight'))return PhysicalProperties.new(d,f,e,fw,ew)end
elseif ptype=='SecurityCapabilities'then return tn(children[1].text)elseif ptype
=='Font'then local family=getText('Family',pnode)or
'rbxasset://fonts/families/SourceSansPro.json'local weightText=getText('Weight',
pnode)local styleText=getText('Style',pnode)local weightMap={['100']=Enum.
FontWeight.Thin,['200']=Enum.FontWeight.ExtraLight,['300']=Enum.FontWeight.Light
,['400']=Enum.FontWeight.Regular,['500']=Enum.FontWeight.Medium,['600']=Enum.
FontWeight.SemiBold,['700']=Enum.FontWeight.Bold,['800']=Enum.FontWeight.
ExtraBold,['900']=Enum.FontWeight.Heavy,['Thin']=Enum.FontWeight.Thin,[
'ExtraLight']=Enum.FontWeight.ExtraLight,['Light']=Enum.FontWeight.Light,[
'Regular']=Enum.FontWeight.Regular,['Medium']=Enum.FontWeight.Medium,['SemiBold'
]=Enum.FontWeight.SemiBold,['Bold']=Enum.FontWeight.Bold,['ExtraBold']=Enum.
FontWeight.ExtraBold,['Heavy']=Enum.FontWeight.Heavy}local weight=weightMap[
tostring(weightText)]or Enum.FontWeight.Regular local style=Enum.FontStyle[
tostring(styleText)]or Enum.FontStyle.Normal return Font.new(family,weight,style
)elseif ptype=='ColorSequence'then local keypoints={}local function 
parseNumbersFromTextNode(node)local text=node.children[1]and node.children[1].
text or''local nums={}for num in text:gmatch('%S+')do table.insert(nums,
tonumber(num)or 0)end return nums end for _,kpNode in ipairs(pnode.children)do
if kpNode.tag=='Keypoint'then local time=tonumber(getText('Time',kpNode))or 0
local r=tonumber(getText('R',kpNode))or 1 local g=tonumber(getText('G',kpNode))
or 1 local b=tonumber(getText('B',kpNode))or 1 table.insert(keypoints,
ColorSequenceKeypoint.new(time,Color3.new(r,g,b)))end end if#keypoints==0 then
local nums=parseNumbersFromTextNode(pnode)for i=1,#nums,5 do if i+3<=#nums then
local time=nums[i]or 0 local r=nums[i+1]or 1 local g=nums[i+2]or 1 local b=nums[
i+3]or 1 table.insert(keypoints,ColorSequenceKeypoint.new(time,Color3.new(r,g,b)
))else break end end end if#keypoints<2 then return ColorSequence.new({
ColorSequenceKeypoint.new(0,Color3.new(1,1,1)),ColorSequenceKeypoint.new(1,
Color3.new(1,1,1))})end return ColorSequence.new(keypoints)elseif ptype==
'NumberSequence'then local keypoints={}local function parseNumbersFromTextNode(
node)local text=node.children[1]and node.children[1].text or''local nums={}for
num in text:gmatch('%S+')do table.insert(nums,tonumber(num)or 0)end return nums
end for _,kpNode in ipairs(pnode.children)do if kpNode.tag=='Keypoint'then local
time=tonumber(getText('Time',kpNode))or 0 local value=tonumber(getText('Value',
kpNode))or 0 local envelope=tonumber(getText('Envelope',kpNode))or 0 table.
insert(keypoints,NumberSequenceKeypoint.new(time,value,envelope))end end if#
keypoints==0 then local nums=parseNumbersFromTextNode(pnode)for i=1,#nums,3 do
if i+1<=#nums then local time=nums[i]or 0 local value=nums[i+1]or 0 local
envelope=nums[i+2]or 0 table.insert(keypoints,NumberSequenceKeypoint.new(time,
value,envelope))else break end end end if#keypoints<2 then return NumberSequence
.new({NumberSequenceKeypoint.new(0,1,0),NumberSequenceKeypoint.new(1,1,0)})end
return NumberSequence.new(keypoints)elseif ptype=='NumberRange'then local 
function parseNumbersFromTextNode(node)local text=node.children[1]and node.
children[1].text or''local nums={}for num in text:gmatch('%S+')do table.insert(
nums,tonumber(num)or 0)end return nums end local minv=tonumber(getText('min'))
local maxv=tonumber(getText('max'))if minv~=nil and maxv~=nil then return
NumberRange.new(minv,maxv)end local nums=parseNumbersFromTextNode(pnode)if#nums
>=2 then return NumberRange.new(nums[1],nums[2])elseif#nums==1 then return
NumberRange.new(nums[1],nums[1])end return NumberRange.new(0,0)elseif ptype==
'SharedString'or ptype=='NetAssetRef'then local key=children[1]and children[1].
text or nil if key and ctx and ctx.sharedStrings and ctx.sharedStrings[key]then
return ctx.sharedStrings[key]end return''elseif ptype=='Rect2D'or ptype=='Rect'
then local minNode=getTagNode('min',pnode)local maxNode=getTagNode('max',pnode)
local minX=minNode and n0(tn(getText('X',minNode)))or 0 local minY=minNode and
n0(tn(getText('Y',minNode)))or 0 local maxX=maxNode and n0(tn(getText('X',
maxNode)))or 0 local maxY=maxNode and n0(tn(getText('Y',maxNode)))or 0 return
Rect.new(minX,minY,maxX,maxY)elseif ptype=='Vector3int16'then return
Vector3int16.new(n0(tn(getText('X'))),n0(tn(getText('Y'))),n0(tn(getText('Z'))))
elseif ptype=='Vector2int16'then return Vector2int16.new(n0(tn(getText('X'))),
n0(tn(getText('Y'))))elseif ptype=='Ray'then local originNode=getTagNode(
'origin',pnode)local dirNode=getTagNode('direction',pnode)local ox,oy,oz,dx,dy,
dz=0,0,0,0,0,0 if originNode then ox=n0(tn(getText('X',originNode)))oy=n0(tn(
getText('Y',originNode)))oz=n0(tn(getText('Z',originNode)))end if dirNode then
dx=n0(tn(getText('X',dirNode)))dy=n0(tn(getText('Y',dirNode)))dz=n0(tn(getText(
'Z',dirNode)))end return Ray.new(Vector3.new(ox,oy,oz),Vector3.new(dx,dy,dz))
elseif ptype=='Faces'then local mask=tn(getText('faces'))or 0 local order={Enum.
NormalId.Right,Enum.NormalId.Top,Enum.NormalId.Back,Enum.NormalId.Left,Enum.
NormalId.Bottom,Enum.NormalId.Front}local args={}for bitIndex,normalId in
ipairs(order)do if bit32.band(mask,bit32.lshift(1,bitIndex-1))~=0 then table.
insert(args,normalId)end end return Faces.new(table.unpack(args))elseif ptype==
'Axes'then local mask=tn(getText('axes'))or 0 local order={Enum.Axis.X,Enum.Axis
.Y,Enum.Axis.Z}local args={}for bitIndex,axis in ipairs(order)do if bit32.band(
mask,bit32.lshift(1,bitIndex-1))~=0 then table.insert(args,axis)end end return
Axes.new(table.unpack(args))elseif ptype=='OptionalCoordinateFrame'then local
cfNode=getTagNode('CFrame',pnode)or getTagNode('CoordinateFrame',pnode)if not
cfNode then return nil end return CFrame.new(n0(tn(getText('X',cfNode))),n0(tn(
getText('Y',cfNode))),n0(tn(getText('Z',cfNode))),n0(tn(getText('R00',cfNode)),1
),n0(tn(getText('R01',cfNode))),n0(tn(getText('R02',cfNode))),n0(tn(getText(
'R10',cfNode))),n0(tn(getText('R11',cfNode)),1),n0(tn(getText('R12',cfNode))),
n0(tn(getText('R20',cfNode))),n0(tn(getText('R21',cfNode))),n0(tn(getText('R22',
cfNode)),1))end warn('Unsupported property type: '..ptype)return nil end local 
function getSourceFromNode(node,refs,ctx)local propNode=getTagNode('Properties',
node)if not propNode then return nil end for _,p in ipairs(propNode.children)do
if p.attrs and p.attrs.name=='Source'then return parseProperty(p.tag,p,refs,ctx)
end end return nil end local function getMeshIdFromNode(node,refs)local propNode
=getTagNode('Properties',node)if not propNode then return nil end for _,p in
ipairs(propNode.children)do if p.attrs and p.attrs.name=='MeshId'then local
value=parseProperty(p.tag,p,refs)if value and value~=''and value~='null'then
return value end end end return nil end local function getTextureIdFromNode(node
,refs)local propNode=getTagNode('Properties',node)if not propNode then return
nil end for _,p in ipairs(propNode.children)do if p.attrs and p.attrs.name==
'TextureID'then local value=parseProperty(p.tag,p,refs)if value and value~=''and
value~='null'then return value end end end return nil end local function 
getAttributesFromNode(node,refs,ctx)local propNode=getTagNode('Properties',node)
if not propNode then return nil end for _,p in ipairs(propNode.children)do if p.
attrs and p.attrs.name=='AttributesSerialize'then local ok,raw=pcall(
parseProperty,p.tag,p,refs,ctx)if ok and raw and raw~=''then return raw end
return nil end end return nil end local function getTagsFromNode(node,refs,ctx)
local propNode=getTagNode('Properties',node)if not propNode then return nil end
for _,p in ipairs(propNode.children)do if p.attrs and p.attrs.name=='Tags'then
local ok,raw=pcall(parseProperty,p.tag,p,refs,ctx)if ok and raw and raw~=''then
return raw end return nil end end return nil end local function 
getRunContextFromNode(node)local propNode=getTagNode('Properties',node)if not
propNode then return nil end for _,p in ipairs(propNode.children)do if p.attrs
and p.attrs.name=='RunContext'then return tonumber(p.children[1].text)end end
end local function getEnumItemFromNode(node,propName,enumObj,default)local
propNode=getTagNode('Properties',node)if not propNode then return default end
for _,p in ipairs(propNode.children)do if p.attrs and p.attrs.name==propName
then local v=tonumber(p.children[1]and p.children[1].text)if v then for _,item
in ipairs(enumObj:GetEnumItems())do if item.Value==v then return item end end
end end end return default end local function rbxmxToInstance(xmlString,debug,
waitInterval)waitInterval=waitInterval or 10000 local tree=parseXML(xmlString)
local root=getTagNode('roblox',tree)or tree.children[1]local refs={}local
scripts={}local pendingRefs={}local instances={}local visited={}local
instanceCount=0 local sharedStrings=parseSharedStrings(root)local ctx={
sharedStrings=sharedStrings}local function create(node)if node.tag~='Item'then
return end local class=node.attrs.class local ref=node.attrs.referent local inst
if class=='Script'then local source=getSourceFromNode(node,refs,ctx)local
runContext=getRunContextFromNode(node)if runContext==Enum.RunContext.Client.
Value then inst=newclientruncontext(source)else inst=newscript(source)end elseif
class=='LocalScript'then local source=getSourceFromNode(node,refs,ctx)if source
then inst=newlocalscript(source)end elseif class=='ModuleScript'then local
source=getSourceFromNode(node,refs,ctx)if source then inst=newmodulescript(
source)end elseif class=='Terrain'then local ok,ws=pcall(game.GetService,game,
'Workspace')inst=ok and ws and ws:FindFirstChildOfClass('Terrain')or nil if not
inst then warn(
[[No Terrain instance available in Workspace; skipping Terrain node]])return nil
end elseif class=='MeshPart'then local meshId=getMeshIdFromNode(node,refs)if not
meshId or meshId==''then warn('No valid MeshId found for MeshPart: '..(node.
attrs.referent or'unknown'))inst=Instance.new('MeshPart')else local
insertService=game:GetService('InsertService')local collisionFidelity=
getEnumItemFromNode(node,'CollisionFidelity',Enum.CollisionFidelity,Enum.
CollisionFidelity.Default)local renderFidelity=getEnumItemFromNode(node,
'RenderFidelity',Enum.RenderFidelity,Enum.RenderFidelity.Automatic)xpcall(
function()inst=insertService:CreateMeshPartAsync(meshId,collisionFidelity,
renderFidelity)local textureId=getTextureIdFromNode(node,refs)if textureId and
textureId~=''then inst.TextureID=textureId end end,function(err)warn(
'Error making meshpart: '..err)inst=Instance.new('MeshPart')end)end else local
ok,result=pcall(Instance.new,class)if ok then inst=result else warn(
"Failed to create instance of class '"..tostring(class)..
[[' (unsupported/deprecated/removed) - substituting a Folder so children/refs still resolve]]
)inst=Instance.new('Folder')inst.Name=class end end if debug then print(
'[RBXMX] Created instance: '..class..' (ref: '..(ref or'no-ref')..')')end refs[
ref]=inst pendingRefs[inst]={}local propNode=getTagNode('Properties',node)if
propNode then for _,p in ipairs(propNode.children)do local ptype=p.tag local
pname=p.attrs.name local mapped=propertyMap[pname]if mapped==false then else
local realPname=mapped or pname:gsub('^%l',string.upper)local parseOk,value=
pcall(parseProperty,ptype,p,refs,ctx)if not parseOk then if debug then warn(
'Failed to parse property: '..realPname..' ('..pname..', type '..ptype..') on '
..class..': '..tostring(value))end value=nil end if value~=nil then if ptype==
'Ref'then pendingRefs[inst][realPname]=value else local success=pcall(function()
inst[realPname]=value end)if not success and debug then warn(
'Failed to set property: '..realPname..' ('..pname..') on '..class)end end end
end end end local attrRaw=getAttributesFromNode(node,refs,ctx)if attrRaw then
local attrs=decodeAttributes(attrRaw,debug)if attrs then for attrName,attrValue
in pairs(attrs)do local setOk=pcall(function()inst:SetAttribute(attrName,
attrValue)end)if not setOk and debug then warn('Failed to set attribute: '..
attrName..' on '..class)end end end end local tagsRaw=getTagsFromNode(node,refs,
ctx)if tagsRaw then for _,tag in ipairs(decodeTags(tagsRaw))do local tagOk=
pcall(function()inst:AddTag(tag)end)if not tagOk and debug then warn(
'Failed to add tag: '..tag..' on '..class)end end end for _,child in ipairs(node
.children)do if child.tag=='Item'then local childInst=create(child)if childInst
then childInst.Parent=inst end end end instanceCount=instanceCount+1 if
waitInterval and instanceCount%waitInterval==0 then task.wait()end return inst
end for _,node in ipairs(root.children)do if node.tag=='Item'then local inst=
create(node)if inst then table.insert(instances,inst)end end end local function 
resolveRefs(inst,visited)if visited[inst]then return end visited[inst]=true for
propName,rbxId in pairs(pendingRefs[inst]or{})do if rbxId=='null'then pcall(
function()inst[propName]=nil end)elseif refs[rbxId]then pcall(function()inst[
propName]=refs[rbxId]end)else warn("Unresolved ref '"..tostring(rbxId).."' for "
..propName..' on '..inst.Name)end end for _,child in ipairs(inst:GetChildren())
do if child:IsA('Instance')then resolveRefs(child,visited)end end end for _,inst
in ipairs(instances)do resolveRefs(inst,visited)end pendingRefs=nil return
instances end return rbxmxToInstance
