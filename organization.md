####  组织机构树，自身关联递归查询树代码

/*
一级组织机构，构建一个list,在主方法里面写一次for循环，
二级以下的组织机构，通过buildTree(parentNode,childrens)递归方法，不断递归，返回parentNode节点。
*/


/**
	 * 构建一棵组织机构树
	 * @return
	 */
	public List<TreeOrganizationVo> findAllWithTree() {
		List<Organization> organizations = findAll();
		List<TreeOrganizationVo> treeOrganizations = new ArrayList<TreeOrganizationVo>();
		
		//构建一级菜单列表 
		for (Organization organization : organizations) {

			//如果是子节点 直接跳过
			if(!StringUtils.isEmpty(organization.getBaseId())) {
				continue;
			}
			
			//如果是根节点 ，进行第一级关联
			//构建当前树，关联子树；
			TreeOrganizationVo firstLevelNode = new TreeOrganizationVo();
			firstLevelNode.setId(organization.getId());
			firstLevelNode.setText(organization.getFullName());
			if(organization.getChildrens() == null || organization.getChildrens().size() < 1) {
				firstLevelNode.setLeaf(true);
			}else {
				//buildTree 递归方法，入参parentNode, organizations集合 返回数据操作后的parentNode
				firstLevelNode = buildTree(firstLevelNode,organization.getChildrens());	
			}
			
			//list 几个一级机构拼接
			treeOrganizations.add(firstLevelNode);
			
		}
		try {
			System.out.println("============json===start==organizations======="+JsonUtil.getJSONString(treeOrganizations)+"=================");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return treeOrganizations;
	}
  
  
  
  
  
	/**
	 * 递归查找 构建一个组织机构树 
	 * @param parentOrganizationVo    父节点Vo(符合json的数据结构)
	 * @param organizations           父节点下距离为1的子节点集合(bean本身的集合)
	 * @return  parentOrganizationVo  父节点Vo                  
	 */
	private TreeOrganizationVo buildTree(TreeOrganizationVo parentOrganizationVo, Set<Organization> organizations) {
	
		for (Organization organization : organizations) {
			
			//构建公有数据id,text
			TreeOrganizationVo treeOrganizationVo = new TreeOrganizationVo();
			treeOrganizationVo.setId(organization.getId());
			treeOrganizationVo.setText(organization.getFullName());

			//如果是叶子节点构建leaf
			if(organization.getChildrens() == null || organization.getChildrens().size() < 1) {
				treeOrganizationVo.setLeaf(true);
				parentOrganizationVo.getChildren().add(treeOrganizationVo);
			}else {
				//如果有子节点 依次增加子节点  构建childen
	//			treeOrganizationVo.setLeaf(false);
				TreeOrganizationVo transChildrenOrganization = buildTree(treeOrganizationVo,organization.getChildrens());
				parentOrganizationVo.getChildren().add(transChildrenOrganization);
				
			}
		}
		return parentOrganizationVo;
	}


