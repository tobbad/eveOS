import glob
import os
import sys

def checkDirEnding(fdir):
    return fdir if fdir[-1] == os.sep else fdir+os.sep

def getSrcFromFolder(srcDirs, srcPattern, trgtDir):
    res=[]
    trgtDir = checkDirEnding(trgtDir)
    for srcF in srcDirs:
        srcF = checkDirEnding(srcF)
        print("Process %s folder" % srcF)
        for f in glob.glob(srcF+srcPattern):
            res.append(trgtDir+f)
    return tuple(res)

def RegisterSrcFolderInEnv(srcDirs, env, trgtDir):
   for srcF in srcDirs:
        srcF = checkDirEnding(srcF)
        env.VariantDir(trgtDir+srcF, srcF, duplicate=0)

debug = False
if ARGUMENTS.get('debug', '0') == '1':
    print "*** Debug build..."
    binFolder = 'bin/Debug/'
    debug = True
else:
    print "*** Release build..."
    binFolder = 'bin/Release/'

srcFolders = ('./core/src/',)
srcFiles = getSrcFromFolder(srcFolders,'*.cpp',binFolder)
srcLib  =   'cut'

testFolders = ('./core/tests/',)
testComFiles= (binFolder+'core/tests/AllTests.cpp',)
testComLib  = 'ctest'

linkLibs = ('CppUTest','CppUTestExt')
libPath  = binFolder
ccDebFlags = '-g'
ccFlags  = '-Wall -std=c++11' + "" if not debug else " %s" % ccDebFlags
incPath  = ('inc/','inc/util')

env = Environment(variant_dir=binFolder,
                  LIBPATH=binFolder,
                  LIBS=(srcLib,testComLib)+linkLibs,
                  CPPPATH=incPath, CCFLAGS=ccFlags)
RegisterSrcFolderInEnv(srcFolders, env, binFolder)
RegisterSrcFolderInEnv(testFolders, env, binFolder)
env.Library(target=binFolder+srcLib, source= srcFiles)
env.Library(target=binFolder+testComLib, source= testComFiles)

for f in glob.glob('core/tests/test_*.cpp'):
    of=f.split(os.sep)[2].split('.')[0]
    env.Program(binFolder+of, (binFolder+f,))
