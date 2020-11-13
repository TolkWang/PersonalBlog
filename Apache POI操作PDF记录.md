# Apache POI操作PDF记录

项目中需要将报表导出成pdf格式文件，采用POI库实现，其中需要实现页眉和页脚功能，记录一下，嘿嘿。

主要就是要写一个自定义的类去继承PdfPageEventHelper这个类，重写其中的方法，就可以啦

class HeaderEventHelper extends PdfPageEventHelper implements Constants
{
	public PdfTemplate header_template;
	public PdfTemplate footer_template;
 	public Font header_font;
 	public Font header_font2;
 	public BaseFont footer_font;
 	public Image header_image;
 	public String order_number;
 	public String attn;
 	public String project;
 	public String description;
 	
 	public HeaderEventHelper(String number, String att, String pro, String desc)
 	{
 		super();
 		order_number = number;
 		attn = att;
 		project = pro;
 		description = desc;
 	}
 	
 	public void onOpenDocument(PdfWriter writer, Document document)
 	{
 		try
 		{
 			header_font = new Font();
 			header_font.setSize(10);
 			header_font2 = new Font();
 			header_font2.setSize(15);
 			header_font2.setStyle("bold");
 			header_template = writer.getDirectContent().createTemplate(400, 100);
 			footer_font = BaseFont.createFont(BaseFont.HELVETICA, BaseFont.WINANSI, BaseFont.EMBEDDED);
 			footer_template = writer.getDirectContent().createTemplate(100, 100);
 			header_image = Image.getInstance(ABSOLUTE_PATH+"images/logo2.png");
 			header_image.setAlignment(Image.NO_BORDER);
 			header_image.scaleToFit(160, 100);
 		}
 		catch (Exception e)
 		{
 			e.printStackTrace();
 		}
 	}
 	
 	public void onStartPage(PdfWriter writer, Document document)
 	{
        super.onStartPage(writer, document);
        

​    try

 		{
        	PdfPTable headertable = new PdfPTable(new float[]{50, 50});
            headertable.getDefaultCell().setBorder(PdfPCell.NO_BORDER);
            PdfPCell cell1 = new PdfPCell(new Phrase("", header_font));
            cell1.disableBorderSide(15);
            cell1.setColspan(2);
            cell1.setFixedHeight(146f);
            headertable.addCell(cell1);
            document.add(headertable);
 		}
 		catch (Exception e)
 		{
 			e.printStackTrace();
 		}
    }
 	
 	public void onEndPage(PdfWriter writer, Document document)
 	{
 		try
 		{
 			PdfPTable header_table = new PdfPTable(new float[]{50, 50});
 			header_table.getDefaultCell().setBorder(PdfPCell.NO_BORDER);
 			header_table.setTotalWidth(document.right() - document.left());
 			PdfPCell header_cell = new PdfPCell(header_image);
 			header_cell.disableBorderSide(15);
 			header_cell.setRowspan(4);
 			header_table.addCell(header_cell);
 			header_table.addCell(new Paragraph("", header_font));
 			header_table.addCell(new Paragraph("", header_font));
 			header_table.addCell(new Paragraph("241 Applewood Cres.Unit 5,Concord,Ontario L4K 4E6", header_font));
 			header_table.addCell(new Paragraph("Tel.(905)760-0804 Fax(905)760-0871", header_font));
 			header_table.writeSelectedRows(0, -1, 30, document.top(), writer.getDirectContent());
 			
 			PdfPTable header_table2 = new PdfPTable(new float[]{50, 50});
 			header_table2.getDefaultCell().setBorder(PdfPCell.NO_BORDER);
 			header_table2.setTotalWidth(document.right() - document.left());
            PdfPCell header_cell2 = new PdfPCell(new Phrase("", header_font));
            header_cell2.disableBorderSide(15);
            header_cell2.setColspan(2);
            header_cell2.setFixedHeight(10f);
            header_table2.addCell(header_cell2);
            header_table2.writeSelectedRows(0, -1, 30, document.top() - header_table.calculateHeights(), writer.getDirectContent());
            

​        PdfPTable header_table3 = new PdfPTable(new float[]{35,30,35});
​        header_table3.getDefaultCell().setBorder(PdfPCell.NO_BORDER);
​        header_table3.setTotalWidth(document.right() - document.left());
​        PdfPCell header_cell3 = new PdfPCell(new Phrase("", header_font2)); 
​        header_cell3.disableBorderSide(15);
​        header_cell3.setFixedHeight(10f);
​        header_cell3.setHorizontalAlignment(Element.ALIGN_RIGHT);
​        header_table3.addCell(header_cell3);
​        header_cell3 = new PdfPCell(new Phrase("PURCHASE ORDER", header_font2)); 
​        header_cell3.disableBorderSide(15);
​        header_cell3.setFixedHeight(10f);
​        header_cell3.setHorizontalAlignment(Element.ALIGN_LEFT);
​        header_table3.addCell(header_cell3);
​        header_cell3 = new PdfPCell(new Phrase("PO# " + order_number, header_font2));
​        header_cell3.disableBorderSide(15);
​        header_cell3.setHorizontalAlignment(Element.ALIGN_RIGHT);
​        header_table3.addCell(header_cell3);
​        header_table3.writeSelectedRows(0, -1, 30, document.top() - header_table.calculateHeights() - header_table2.calculateHeights(), writer.getDirectContent());
​        header_table2.writeSelectedRows(0, -1, 30, document.top() - header_table.calculateHeights() - header_table2.calculateHeights() - header_table3.calculateHeights(), writer.getDirectContent());
​        PdfPTable header_table4 = new PdfPTable(new float[]{35,15,50});
​        header_table4.getDefaultCell().setBorder(PdfPCell.NO_BORDER);
​        header_table4.setTotalWidth(document.right() - document.left());
​        PdfPCell header_cell4 = new PdfPCell(new Phrase("", header_font)); 
​        header_cell4.disableBorderSide(15);
​        header_cell4.setFixedHeight(15f);
​        header_cell4.setHorizontalAlignment(Element.ALIGN_RIGHT);
​        header_table4.addCell(header_cell4);
​        header_cell4 = new PdfPCell(new Phrase("ATTN: " + attn, header_font)); 
​        header_cell4.disableBorderSide(15);
​        header_cell4.setFixedHeight(15f);
​        header_cell4.setHorizontalAlignment(Element.ALIGN_LEFT);
​        header_table4.addCell(header_cell4);
​        header_cell4 = new PdfPCell(new Phrase("Project: " + project, header_font)); 
​        header_cell4.disableBorderSide(15);
​        header_cell4.setFixedHeight(15f);
​        header_cell4.setHorizontalAlignment(Element.ALIGN_RIGHT);
​        header_table4.addCell(header_cell4);
​        header_cell4 = new PdfPCell(new Phrase("Description: " + description, header_font));
​        header_cell4.setColspan(3);
​        header_cell4.disableBorderSide(15);
​        header_cell4.setFixedHeight(20f);
​        if(description.length() > 0)
​        {
​        	header_cell4.setHorizontalAlignment(Element.ALIGN_RIGHT);
​        }
​        else
​        {
​        	header_cell4.setHorizontalAlignment(Element.ALIGN_CENTER);
​        }
​        header_table4.addCell(header_cell4);
​        header_table4.writeSelectedRows(0, -1, 30, document.top() - header_table.calculateHeights() - header_table2.calculateHeights() - header_table3.calculateHeights() - header_table2.calculateHeights(), writer.getDirectContent());
 		
 		PdfContentByte cb = writer.getDirectContent();
 		cb.setFontAndSize(footer_font, 10);
 		cb.saveState();
 		cb.beginText();
 		float y = document.bottom(-25);
 		cb.showTextAligned(PdfContentByte.ALIGN_CENTER,"Page "+writer.getPageNumber()+" of",(document.right() + document.left())/2,y,0);
 		float textSize = footer_font.getWidthPoint("Page "+writer.getPageNumber()+" of", 12);
 		cb.endText();
 		cb.addTemplate(footer_template, (document.right() + document.left())/2+(textSize/2),y);
 		cb.restoreState();

 		}
 		catch (Exception e)
 		{
 			e.printStackTrace();
 		}
 	}
 	
 	public void onCloseDocument(PdfWriter writer, Document document)
 	{
 		footer_template.beginText();
 		footer_template.setFontAndSize(footer_font, 10);
 		footer_template.showText(Integer.toString(writer.getPageNumber()));
 		footer_template.endText();
 		footer_template.closePath();
 	}
}