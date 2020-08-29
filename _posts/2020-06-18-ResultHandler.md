---
title: 대용량 데이터 조회 - ResultHandler
updated: 2020-06-18 09:56
category: Java
---

> ## 소스 현행화 필요함

대용량 데이터 처리 시 사용하는 스트리밍 방식
1. PreparedStatement, ResultSet 사용
2. apache 제공 ResultHandler 사용   
   
ResultSet과 비교해보면 속도면에서 큰 차이는 없으나, SQL을 직접 작성하는 ResultSet 활용 방식과는 다르게 ResultHandler는 MyBatis로 DB 접근 가능함. 장점이 많아 보였지만 끝내 사용한 방식은 PreparedStatement, ResultSet을 활용..


### 1. ResultHandler 인터페이스 구현 (Handler)
```java
package o2.bai.sheet.cmmn.util;

import java.util.Map;

import org.apache.ibatis.session.ResultContext;
import org.apache.ibatis.session.ResultHandler;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.streaming.SXSSFCell;
import org.apache.poi.xssf.streaming.SXSSFRow;
import org.apache.poi.xssf.streaming.SXSSFSheet;
import org.apache.poi.xssf.streaming.SXSSFWorkbook;

public class ExcelHandler implements ResultHandler {

	private SXSSFWorkbook workbook;
	private SXSSFSheet sheet;
	private SXSSFRow row;
	private int rowNum = 0;

	private String dataNm;
	private int dataCnt;

	private String[] korColNm;
	private String[] engColNm;
	
	public ExcelHandler(SXSSFWorkbook workbook, Map<String, Object> param) {
		
		this.workbook = workbook;
		this.korColNm = (String[]) param.get("KOR_COL_NM");
		this.engColNm = (String[]) param.get("ENG_COL_NM");
		this.dataNm = (String) param.get("KOR_TABLE_NM");
		this.dataCnt = (Integer) param.get("DATA_CNT");
		
		createFirstSheet(this.workbook);
		writeTitle(sheet, rowNum);
		
	}
	
	private SXSSFSheet createFirstSheet (Workbook workbook) {
		return sheet = (SXSSFSheet) workbook.createSheet(dataNm);
	}
	
	private void writeTitle(SXSSFSheet sheet, int rowNum) {
		SXSSFCell cell = null;
		int cellIdx = 0;
		
		if(korColNm != null && korColNm.length > 0) {
			row = sheet.createRow(rowNum);
			
			for(int i = 0; i < korColNm.length; i++) {
				cell = row.createCell(cellIdx++);
				cell.setCellValue(korColNm[i]);
			}
		}
		this.rowNum = rowNum + 1;
	}
	
	private void createBody(Map<String, Object> data) {
		SXSSFCell cell = null;
		int cellIdx = 0;
		
		row = sheet.createRow(rowNum++);
		
		if( engColNm != null && engColNm.length > 0 ) {
			for(int i = 0; i < engColNm.length; i ++) {
				cell = row.createCell(cellIdx++);
				cell.setCellValue( data.get(engColNm[i]) == null ? "" : String.valueOf(data.get(engColNm[i])) );
			}
		}
	}
	
	@Override
	public void handleResult(ResultContext resultContext) {
		Map data = (Map)resultContext.getResultObject();
		createBody(data);
	}

}
```
  
  
### 2. 구현한 클래스를 사용 (Service)
```java
package o2.bai.sheet.bass.service;

import java.io.File;
import java.io.FileOutputStream;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import javax.annotation.Resource;

import org.apache.poi.xssf.streaming.SXSSFWorkbook;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.stereotype.Service;

import com.ibm.icu.text.SimpleDateFormat;

import o2.bai.sheet.cmmn.util.ExcelHandler;
import o2.bai.sheet.cmmn.util.SheetUtil;
import o2.bai.sheet.cmmn.util.ZipUtil;

@Service("excelDownloadService")
public class ExcelDownloadService {

	@Resource(name = "sqlSession")
	private SqlSessionTemplate sqlSession;
	
	
	/**
	 * 엑셀 데이터 건수에 따라 분할 조회, 또는 일괄 조회
	 * @param dataList - 다운받을 데이터 목록
	 * @return File - 엑셀 파일, 또는 압축 파일의 상위 디렉토리
	 */
	public File writeData(List<Map<String, Object>> param) throws Exception{
		
		Date startDt = new Date();
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyMMddHHmmss");
		String strStartDt = dateFormat.format(startDt);

		final int LIMIT_COUNT =  Integer.parseInt(AppConfigure.getInstance().getValue("sheet.download.write.limit"));
		
		String tmpPath = System.getProperty("java.io.tmpdir");
		String topPath = AppConfigure.getInstance().getValue("sheet.download.root");
		String userId = SheetUtil.getUserId();
		final String SEP = File.separator;
		File repo = new File(tmpPath + SEP + topPath + SEP + userId + SEP + strStartDt);
		if(!repo.exists()) { repo.mkdirs(); }

		FileOutputStream fos = null;
		SXSSFWorkbook xlsxWorkbook = null;
		
		for(Map<String, Object> data : param) {
			
			int dataCnt = (Integer) data.get("RECORD_CNT"); // 현재 데이터의 카운트
			int repeatCnt = dataCnt % LIMIT_COUNT > 0 ? dataCnt / LIMIT_COUNT + 1 : dataCnt / LIMIT_COUNT // 300,000 건 초과 시 n + 1회 반복
			
			List<Map<String, Object>> colMetaList = sqlSession.selectList("data.meta.getUsrColMeta", param);
			String[] korColNm = new String[colMetaList.size()];
			String[] engColNm = new String[colMetaList.size()];

			// column name			
			for(int i = 0; i < colMetaList.size(); i ++) {
				korColNm[i] = colMetaList.get(i).get("KOR_COL_NM").toString();
				engColNm[i] = colMetaList.get(i).get("ENG_COL_NM").toString();
			} 
			
			// excel write process
			for( int i = 0; i < repeatCnt; i++ ) {
				
				data.put("KOR_COL_NM", korColNm);
				data.put("ENG_COL_NM", engColNm);
				data.put("START_INDEX", i * LIMIT_COUNT + 1);
				data.put("END_INDEX", i * LIMIT_COUNT + LIMIT_COUNT;
				data.put("DATA_CNT", dataCnt);
				
				xlsxWorkbook = getData(data);
				
				String prefix = repeatCnt > 1 ? data.get("KOR_TABLE_NM").toString() + "_" + (i+1) : data.get("KOR_TABLE_NM").toString();
				File xlsxFile = new File(repo, prefix + ".xlsx");
				fos = new FileOutputStream(xlsxFile);
				
				workbook.write(fos);
				workbook.dispose();
				fos.close();
				
			} // end for
				
		}
		
		return repository;
	}
	
	/**
	 * 실제 데이터 테이블을 조회한 후 SpreadSheet 객체를 만들어 반환
	 * @param param - 데이터 정보(DATASET_ID, DATA_SE, KOR_COL_NM, ENG_COL_NM ...)
	 * @return Workbook - 스프레드 시트 객체
	 */
	public SXSSFWorkbook getData(Map<String, Object> param) {		

		// Workbook		
		SXSSFWorkbook workbook = new SXSSFWorkbook(10000); // 10000 레코드 까지만 메모리에 상주, 나머지는 디스크를 이용
		
		// Result Handler
		ExcelHandler excelHandler = new ExcelHandler(workbook, param);
		
		// access & write data
		sqlSession.select("bass.xlsx.getDataForXlsx", param, excelHandler);
		
		return xlsxWorkbook;
	}
	
	/**
	 * 지정된 경로의 파일 개수를 확인하여 2개 이상일 경우 압축
	 * @param dir - 압축 여부 확인할 디렉토리 경로
	 * @return String - 다운로드 할 파일의 URL
	 */
	public String zipData(File dir) throws Exception {
		
		String downloadURL = null; // 다운로드 할 파일의 경로
		
		if( dir.listFiles().length > 1 ) { // 디렉토리의 파일이 2개 이상일 경우 - 압축 대상
			
			List<String> fileList = new ArrayList<String>();	// 압축 대상(파일, 폴더) 리스트
			String zipDir = dir.getAbsolutePath() + "/" + SheetUtil.getUserId() + ".zip";		// 생성될 압축파일의 경로
			String targetFolder = dir.getAbsolutePath();		// 압축할 상위 폴더
			
			ZipUtil.generateFileList(new File(targetFolder), fileList, targetFolder);
			ZipUtil.zipIt(zipDir, targetFolder, fileList);
			
			downloadURL = "/bai.sheet/bass/excel/" + dir.getName() + ".zip.do";
			
		} else { // 디렉토리의 파일이 1개일 경우 - 압축 X
			
			downloadURL = "/bai.sheet/bass/excel/" + dir.listFiles()[0].getName() + ".do";
			
		}
		
		
		return downloadURL;
	}
	
}
```